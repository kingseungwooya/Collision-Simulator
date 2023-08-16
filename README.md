# Collision Simulator Flow

## 프로젝트 소개

---

본 프로젝트는 AIS Data와 중앙 해양 심판원의 재결서를 바탕으로 충돌 상황을 Simulating 해준다. 

> 데이터들은 연구소 보안상 공유 불가능합니다.
> 

## 사용 방법

---

<aside>
💡  이 프로그램을 이용하기 위해서는 아래와 같은 Data들이 준비되어 있어야 합니다. Data를 아래와 같은 경로에 배치해주세요.

</aside>

- 각 Data 의 shape는 각 경로에 있는 README.md를 참고해주세요
    - data/dynamic_ais -> 사고가 발생한 날짜의 동적 AIS DATA
    - data/static_ais -> 선박의 정적 AIS DATA
    - data/reconciliation -> 해심원의 재결서에 나온 위치, 시간 데이터

Data를 옳바른 경로에 위치하였다면 본인의 환경에 맞게 ipynb 코드를 실행하면 됩니다.!

## 기능명세서

---

> 코드를 먼저 짜기 앞서 필요한 기능들을 나열하고 기능 단위로 개발을 한다.
> 
- 재결서 정보를 가져오는 기능
- 재결서 정보를 토대로 사고 정보(위치, 시간)을 추출하는 기능
- 사고 정보를 기반으로 AIS Data를 추출하는 기능
- 추출한 Data중 사고가 발생한 두 선박을 찾는 기능
- 추출한 Data를 csv파일로 생성하는 기능
- 추출한 Data를 지도에 시각화 하는 기능

`find_data_from_reconciliation` : “재결서” 바탕으로 사고 정보를 추출한다.

- `convert_degrees` : 도, 분, 초로 되어있는 위치 정보를 위도 경도로 바꿈
- `check_digit_length` : 날짜를 추출하기 위한 함수

`impot_ais_data` : 사고 정보와 User Input을 바탕으로 데이터를 불러와 Dataframe에 저장한다. 

`find_target_vessel` : 사고 선박을 찾기 위해 여러 Filter를 가지고 있는 함수이다.

- `calc_time_range` : 사고 시간으로부터 추출 간격을 정한다.
- **`find_by_recursion`:**
    
    > 사고 난 두 선박을 찾기 위해 사고 지점의 범위와 시간을 재귀를 통해 점점 줄여나가며, 순회하는 동안 가장 Data의 빈도수가 높은 두 개의 선박을 찾아낼 때까지. 재귀적인 호출을 이어나간다.
    > 
    
- `df_to_csv` : 재귀함수로 찾은 결과물에 해당하는 data들을 csv파일로 generate 한다.

`draw_map` : folium 라이브러리를 통해 OSM 기반 MAP을 생성한다.

- `create_legend` : legend 를 형성해 Marker의 정보를 나타냅니다
    - `find_ship_type` : 앞서 찾은 두 선박의 동적데이터의 MMSI를 이용해 AIS 정적 데이터를 찾습니다.
- `convert_to_line` : AIS Data들을 선으로 이어주는 역할을 합니다.
    - `random_color` : 각 MMSI 별 고유한 랜덤 색깔을 지정해줍니다. (항로 표시)