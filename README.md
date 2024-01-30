# Enefit ⚡
## 대회 설명

📌참고: 에너지 프로슈머는 에너지를 소비하고 생산하는 개인, 기업 또는 조직을 말합니다. 이 개념은 소비자가 단순히 유틸리티 업체로부터 에너지를 구매하고 중앙집중식 발전원에 의존하는 기존 모델에서 벗어나는 것을 의미합니다. 에너지 프로슈머는 일반적으로 태양광 패널(또는 풍력 터빈, 소규모 수력 발전 등)과 같은 재생 에너지원을 통해 자체적으로 전기를 생산함으로써 에너지 생태계에 적극적으로 참여합니다. 또한 자체 발전량이 필요량을 충족하기에 부족할 경우 전력망에서 에너지를 소비합니다.

- 운영 비용 증가, 잠재적인 전력망 불안정성, 에너지 자원의 비효율적 사용 등 에너지 불균형이 심화되면서 프로슈머의 수가 빠르게 증가하고 있습니다.
- 이 대회의 목표는 에너지 불균형 비용을 줄이기 위한 프로슈머의 에너지 예측 모델을 만드는 것입니다.
- 이 문제가 해결되면 불균형 비용을 줄이고 전력망의 신뢰성을 개선하며 프로슈머를 에너지 시스템에 보다 효율적이고 지속 가능하게 통합할 수 있습니다.
- 또한, 더 많은 소비자들이 프로슈머가 되어 재생 에너지 생산과 사용을 촉진하도록 장려할 수 있습니다.


## 데이터 설명
📌 참고:
이번 대회의 과제는 태양광 패널을 설치한 에스토니아 에너지 고객이 생산하고 소비하는 전기량을 예측하는 것입니다. 날씨 데이터, 관련 에너지 가격, 설치된 태양광 발전 용량 기록에 액세스할 수 있습니다.
시계열 API를 이용한 예측 대회입니다. 비공개 순위표는 제출 기간이 마감된 후 수집된 실제 데이터를 사용하여 결정됩니다.


## 파일¶
### train.csv

- county - county의 ID 코드입니다.
- is_business - 프로슈머가 비즈니스인지 여부를 나타내는 Boolean 입니다.
- product_type - 다음과 같이 계약 유형에 코드가 매핑된 ID 코드입니다.: {0: "Combined", 1: "Fixed", 2: "General service", 3: "Spot"}.
- target - 해당 시간 동안의 해당 세그먼트에 대한 소비 또는 생산량입니다. 세그먼트는 다음과 같이 정의됩니다 county, is_business, and product_type.
- is_consumption - 이 행의 대상이 소비인지 생산인지에 대한 Boolean 입니다.
- datetime - 에스토니아 시간(Estonian time) in EET (UTC+2) / EEST (UTC+3).
- data_block_id - 동일한 data_block_id를 공유하는 모든 행은 동일한 예측 시간에 사용할 수 있습니다. 이는 매일 오전 11시에 실제로 예보가 만들어질 때 어떤 정보를 사용할 수 있는지에 대한 함수입니다. 예를 들어, 10월 31일 예보에 대한 예보 날씨 data_block_id가 100이면 10월 31일의 과거 날씨 data_block_id는 101이 되며, 과거 날씨 데이터는 실제로 다음 날에만 사용할 수 있기 때문입니다.
- row_id - 행의 고유 식별자입니다.
- prediction_unit_id - county, is_business 및 product_type 조합에 대한 고유 식별자입니다. 테스트 세트에 새로운 예측 단위가 나타나거나 사라질 수 있습니다. gas_prices.csv

- origin_date - 하루 전 가격을 사용할 수 있게 된 날짜입니다.

- forecast_date - 예상 가격이 관련되어야 하는 날짜입니다.
[lowest/highest]_price_per_mwh - 해당 거래일 전일 시장에서 거래되는 천연가스의 최저/최고 가격(메가와트시당 유로화 환산)입니다.
data_block_id

### client.csv

- product_type
- county - county'의 ID 코드입니다. ID 코드를 카운티 이름에 매핑하는 방법은county_id_to_name_map.json`을 참조하세요.
- eic_count - 집계된 소비 포인트 수 (EICs - European Identifier Code).
- installed_capacity - 설치된 태양광 패널 용량(킬로와트).
- is_business - 프로슈머가 비즈니스인지 여부를 나타내는 Boolean 입니다.
- date
- data_block_id


### electricity_prices.csv
- origin_date
- forecast_date
- euros_per_mwh - 다음 날의 전력 가격은 메가와트시당 유로로 표시됩니다.
- data_block_id

### forecast_weather.csv 
- W예측 시점에 사용할 수 있었던 일기 예보입니다. 다음으로 부터 가져온 자료입니다. 유럽 중기 예보 센터.

- [latitude/longitude] - 일기 예보의 좌표입니다
- origin_datetime - 예측이 생성된 시점의 타임스탬프입니다.
- hours_ahead - 예보 생성과 예보 날씨 사이의 시간 수입니다. 각 예보는 총 48시간 동안 적용됩니다.
- temperature - 지상 2m 지점의 대기 온도를 섭씨 단위로 표시합니다.
- dewpoint - 지상 2m 지점에서의 이슬점 온도를 섭씨 단위로 표시합니다.
- cloudcover_[low/mid/high/total] - 다음 고도 구간에서 구름으로 덮인 하늘의 비율입니다: 0~2km, 2~6, 6+ 및 합계.
- 10_metre_[u/v]_wind_component - 풍속의 [동쪽/북쪽] 성분은 지표면 10미터 상공에서 측정한 풍속을 초당 미터 단위로 표시합니다.
- data_block_id
- forecast_datetime - 예상 날씨의 타임스탬프입니다. origin_datetime에 hours_ahead을 더하여 생성됩니다.
- direct_solar_radiation - 태양의 방향에 수직인 평면에서 지표면에 도달한 직사광선이 이전 시간 동안 누적된 것으로, 평방미터당 와트시 단위입니다.
- surface_solar_radiation_downwards - 지구 표면의 수평면에 도달하는 직접 및 확산 태양 복사열을 평방미터당 와트시 단위로 나타낸 값입니다.
- snowfall - 지난 한 시간 동안의 적설량(미터 환산) 단위입니다.
- total_precipitation - 이전 한 시간 동안 지구 표면에 내린 비와 눈으로 구성된 축적된 액체를 미터 단위로 표시합니다.

### historical_weather.csv 
- 과거 날씨 데이터.
- datetime
- temperature
- dewpoint
- rain - 예보 규칙(forecast conventions)과 다릅니다. 이전 한 시간 동안의 대규모 기상 시스템에서 내린 비(밀리미터 단위).
- snowfall - 예보 규칙과 다릅니다. 이전 시간 동안의 적설량(센티미터)입니다.
- surface_pressure - 표면의 기압을 헥토파스칼 단위로 표시합니다.
- cloudcover_[low/mid/high/total] - 예보 규칙과 다릅니다. 0~3km, 3~8, 8+ 및 총 구름 덮개입니다.
- windspeed_10m - 예보 규칙과 다릅니다. 지상 10미터에서의 풍속(초당 미터)입니다.
- winddirection_10m - 예보 규칙과 다릅니다. 지상 10미터에서의 풍향(도)입니다.
- shortwave_radiation - 예측 규칙과 다릅니다. 평방미터당 와트시 단위의 전 세계 수평 조사입니다.
- direct_solar_radiation
- diffuse_radiation - 예측 규칙과 다릅니다. 평방미터당 와트시 단위의 확산 일사량입니다.
- [latitude/longitude] - 기상 관측소의 좌표입니다.
- data_block_id

### public_timeseries_testing_util.py 
- 사용자 지정 오프라인 API 테스트를 더 쉽게 실행하기 위한 선택적 파일입니다. 자세한 내용은 스크립트의 문서 문자열을 참조하세요. 사용하기 전에 이 파일을 편집해야 합니다.

### example_test_files/ 
- API의 작동 방식을 설명하기 위한 데이터입니다. API가 제공하는 것과 동일한 파일과 열을 포함합니다. 처음 세 개의 data_block_ids는 훈련 집합의 마지막 세 개의 data_block_ids의 반복입니다.

### example_test_files/sample_submission.csv 
- API를 통해 전달된 유효한 샘플 제출물. 이 노트북을 참조하여 샘플 제출을 사용하는 아주 간단한 예시를 확인하세요.

### example_test_files/revealed_targets.csv 
- 실제 목표 값으로, 하루의 지연을 두고 제공됩니다.

### enefit/ 
- API를 활성화하는 파일. API는 15분 이내에 모든 행을 제공하고 0.5GB 미만의 메모리를 예약할 것으로 예상합니다. 다운로드할 수 있는 API 사본은 example_test_files/의 데이터를 제공합니다. API를 발전시키기 위해서는 해당 날짜에 대한 예측을 해야 하지만, 이러한 예측은 점수가 매겨지지 않습니다. 처음에는 약 3개월의 데이터가 제공되며, 예측 기간이 끝날 때까지 최대 10개월의 데이터가 제공될 것으로 예상됩니다.
