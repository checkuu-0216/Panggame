# 🎮 Pang Game (Python + Pygame)
## 📌 소개
이 프로젝트는 Python의 pygame 라이브러리를 활용해 제작한 클래식 팡(Pang) 게임입니다.
캐릭터를 좌우로 움직이며 무기를 발사해 화면에 튀어다니는 풍선을 모두 제거하면 게임 클리어, 풍선에 닿거나 시간이 초과되면 게임 오버가 됩니다.

![image](https://github.com/user-attachments/assets/4edbb09c-56e3-4ab5-b036-7afd2429aaee)

## 📷 게임 화면 구성
배경 (background.png)

스테이지 (stage.png): 캐릭터가 서는 발판 역할

캐릭터 (character.png): 좌우 이동, 무기 발사 가능

무기 (weapon.png): 위로 날아가며 공과 충돌 시 공 분열

공 (balloon1.png ~ balloon4.png): 점점 작아지며 분열

## 🕹️ 조작 방법
← : 캐릭터 왼쪽 이동

→ : 캐릭터 오른쪽 이동

Space : 무기 발사

## 🧠 핵심 기능
다수의 무기 발사 가능

공 충돌 시 분열 기능 구현

캐릭터, 무기, 공 간의 충돌 처리

시간 제한(100초) 기능 포함

## 🛠️ 공 분열 관련 버그 수정
마지막 크기(balloon4.png)로 분열 시
정상적으로 두 개의 공이 생성되지 않고, 작은 크기 공 하나만 분열 되고 기존의 공은 그대로 움직이는 오류가 있었습니다.

![스크린샷 2025-04-17 113100](https://github.com/user-attachments/assets/75ff0d23-101f-4cc2-b29c-3441aa73c01c)


## 🔧 수정 내용
<details>
  <summary>수정 코드</summary>
<pre> 
#공과 무기들 충돌처리
        for weapon_idx, weapon_val in enumerate(weapons):
            weapon_pos_x = weapon_val[0]
            weapon_pos_y = weapon_val[1]
            
            #무기 rect 정보 업데이트
            weapon_rect = weapon.get_rect()
            weapon_rect.left = weapon_pos_x
            weapon_rect.top = weapon_pos_y

            #충돌 체크
            if weapon_rect.colliderect(ball_rect):
                weapon_to_remove = weapon_idx # 해당 무기없애기 위한 설정
                ball_to_remove = ball_idx # 해당 공 없애기 위한 값 설정

                if ball_img_idx < 3 :
                    #현재 공 크기 정보 가져옴
                    ball_width = ball_rect.size[0]
                    ball_height = ball_rect.size[1]
                    #나눠진 공 정보
                    small_ball_rect = ball_images[ball_img_idx + 1].get_rect()
                    small_ball_width = small_ball_rect.size[0]
                    small_ball_height = small_ball_rect.size[1]

                    # 왼쪽으로 튕겨나가는 작아진 공
                    balls.append({
                        "pos_x" : ball_pos_x + (ball_width / 2) - (small_ball_width / 2), # 공의 x 좌표
                        "pos_y" : ball_pos_y + (ball_height / 2) - (small_ball_height / 2), # 공의 y 좌표
                        "img_idx" : ball_img_idx + 1, # 공의 이미지 인덱스
                        "to_x" : -3, #공의 x 축 이동방향
                        "to_y" : -6, # y축 이동 방향
                        "init_spd_y" : ball_speed_y[ball_img_idx + 1] #y 최초 속도
                    })
                    # 오른쪽으로 튕겨나가는 작아진 공
                    balls.append({
                         "pos_x" : ball_pos_x + (ball_width / 2) - (small_ball_width / 2), # 공의 x 좌표
                        "pos_y" : ball_pos_y + (ball_height / 2) - (small_ball_height / 2), # 공의 y 좌표
                        "img_idx" : ball_img_idx + 1, # 공의 이미지 인덱스
                        "to_x" : 3, #공의 x 축 이동방향
                        "to_y" : -6, # y축 이동 방향
                        "init_spd_y" : ball_speed_y[ball_img_idx + 1]
                    })
                break
        else: # 계속 게임 진행
            continue # 안쪽 for 문 조건이 맞지 않으면 continue 바깥 for문 계속 진행
        break # 안쪽 for문에서 break를 만나면 여기로 이동 후 break로 바깥for문도 종료
</pre> </details>
바깥 공 반복문을 종료하도록 하여 불필요한 루프 진행을 막고 정확하게 두 개의 작은 공을 생성합니다.

## 🧾 설치 및 실행 방법
 Pygame 설치
<pre>pip install pygame</pre>

프로젝트 구조
<pre>pang_game/
├── images/
│   ├── background.png
│   ├── stage.png
│   ├── character.png
│   ├── weapon.png
│   ├── balloon1.png
│   ├── balloon2.png
│   ├── balloon3.png
│   └── balloon4.png
├── pang_game.py</pre>

게임 실행
<pre>python pang_game.py</pre>

## 🎯 향후 개선 아이디어
점수 시스템 추가

게임 난이도 설정 (속도 조절, 공 개수 조정 등)

캐릭터와 배경 애니메이션 추가

사운드 효과 적용

## 📝 제작자 코멘트
이 게임은 pygame 학습을 위한 실습용으로 제작되었으며,
객체 충돌 처리 및 다중 발사 로직 구현 등 기본적인 게임 시스템 구현에 중점을 두었습니다.
특히, 공 분열 처리 로직의 루프 제어를 정확히 처리하는 것이 핵심 포인트였습니다.
