# 디스플레이 센서의 불량품 요인 분석

- 2016/1/1~12/31의 Display Sensor Data를 이용하여 부품폐기의 요인이 되는 sensor 5개를 찾아야함.
- 팀원 : 구혜진, 김민기, 서영흔, 이세희,  이호영, 황정석
- 기간 : 2021/7/26 ~ 2021/8/
- [directory 구조](#directory-구조)
- [이용 방법](#이용-방법)

<br>

## 1주차 - 데이터 탐색
- 2021/07/26~ 2021/7/29
- 결측치처리
- 상관관계확인
- 다중공선성 확인
- 상관관계가 1.0인 데이터를 제거하여 다중공선성확인함
- 일부 데이터의 기술통계량 확인
- 컬럼명이 비슷한 것끼리 그룹화하여 상관관계확인해보기



### <금요일까지 각자 하기>

1. 상관관계 1로 된것만 해보기
2. 분산이 0인 컬럼은 제거하기

### <일요일 오전까지  각자 하기>

1. Left, Right, L, R에 따라서 컬럼 나누기
    나중에 폐기율 LEFT RIGHT 나눠서 분류를 해보기 위해 
2. 주성분 분석해서 산점도 그려보기



1. PPT작성 ??

### <목요일 까지>

1. VIF계수 구해서 다중공선성이 가장 높은 것을 제거 
   -> 이 과정을 계속 반복. 다중공선성이 30이하가 될때까지
2. 상관관계 0.x로 줄여서 VIF계수구하기
3. 시계열
4. 종속변수 자체를 연관분석, 시계열분석 해보기 (큰영향은 안 줌)
5.  분류 모델로 만드는 것이 가장 좋음!
   1. 이진화 하여 값이 어느 정도로 나오는 것이 좋을지 찾아보자


## directory 구조
   ```
   📁 data
   ├── 📄 factory_all_columns.txt
   |		모든 컬럼명 저장(date,  폐기율 관련 컬럼 포함)
   ├── 📄 same_coef_frame.xlsx
   |		상관관계가 1.0인 컬럼명 저장파일
   📑 1_Display_Sensor_Anomaly_Analysis_결측치처리와 상관계수저장.ipynb
   |		결측치처리와 상관계수정보를  csv파일로 저장하는 파일
   📑 2_Display_Sensor_Anomaly_Analysis_저장데이터읽기.ipynb
   |		저장된 csv파일들을 불러와 VIF계수 및 그 다음 로직처리를 구현할 파일
   📑 Display_Sensor_Anomaly_Analysis.ipynb
   		7/29 멘토링때 발표한 파일
   ```



## 이용 방법

1. zip파일을 다운로드
   압축을 해제하여 .ipynb파일과 나무플래닛으로부터 받은 원본데이터(factory_glass_2016.csv)을  working directory에 위치시키기

2. 1_Display_Sensor_Anomaly_Analysis_결측치처리와 상관계수저장.ipynb    

   결측치처리와 상관계수정보를  csv파일로 저장하는 파일입니다. 이후 처리시 csv파일을 불러와 사용함으로써 시간을 절약할 수 있습니다.   

   잘안되는 부분이 있거나 이상한 부분이 있으면 연락주세요!   

   ​    

   - 실행환경에 맞게 working_dir 수정

   ```
   working_dir = '/content/drive/MyDrive/Colab Notebooks/data/Display_Sensor_Anomaly_Analysis/'
   ```

   - 모든 sensor컬럼 데이터간의 상관계수로부터 추출할 compare_num변수를 변경

     - 0.9 혹은 1.0 등으로  변경해보면 됩니다 

     ```
     # 추출할 상관계수 크기 
     compare_num = 1.0
     
     high_coef_df = pd.DataFrame(columns=['col1','col2','coef'])
     def get_high_coef(coef_df, compare_num):
         coef_df = coef_df[coef_df['coef'] >= compare_num]
         global high_coef_df
         high_coef_df = high_coef_df.append(coef_df,ignore_index=True)
         
     get_high_coef(coef_list_1,compare_num)
     get_high_coef(coef_list_2,compare_num)
     get_high_coef(coef_list_3,compare_num)
     get_high_coef(coef_list_4,compare_num)
     get_high_coef(coef_list_5,compare_num)
     get_high_coef(coef_list_6,compare_num)
     
     file_name = 'high_coef_df_upper_' + str(compare_num)
     # csv 파일로 저장
     high_coef_df.to_csv(working_dir + file_name + '.csv')
     
     ```

   - 1_Display_Sensor_Anomaly_Analysis_결측치처리와 상관계수저장.ipynb 모두 실행하기

     - 파일을 실행하고 나면 working_directory에 다음과 같은 파일이 생성됩니다

       \- 전체 데이터 : fact_data.csv

       \- sensor 데이터 : sensor_data.csv

       \- 폐기율 관련 데이터 : trash_data.sv

       \-  컬럼별 모든 상관계수정보 

       ```
       corr_df0-100.csv
       corr_df100-200.csv
       corr_df200-300.csv
       corr_df300-500.csv
       corr_df500-700.csv
       corr_df700-891.csv
       ```
       \- XX 이상의 상관계수 정보 :high_coef_df_upper_XX.csv

<br>

4. 2_Display_Sensor_Anomaly_Analysis_저장데이터읽기.ipynb       

   3의 실행으로 저장된 파일들을 불러와 VIF계수를 확인할 수 있습니다.      

   - 실행환경에 맞게 working_dir 수정

   ```
   working_dir = '/content/drive/MyDrive/Colab Notebooks/data/Display_Sensor_Anomaly_Analysis/'
   ```

   - 3에서 저장했던 high_coef_df_upper_XX.csv 파일을 읽어오기 위해 compare_num을 수정합니다.

     ```
     compare_num = 1.0
     
     file_name = 'high_coef_df_upper_' + str(compare_num)
     
     # csv 파일불러오기
     high_coef_df = pd.read_csv(working_dir + file_name + '.csv',usecols=(1,2,3))
     high_coef_df
     ```

   - 이 부분은 sensor컬럼에 대한 VIF계수를 구하는 처리입니다.

     - columns_all의 변수를 원하는 상황에 맞게 주석해제합니다
     - 이 부분은 **실행시간이 약 40분 소요**됩니다.

     ```
     from statsmodels.stats.outliers_influence import variance_inflation_factor
     
     # 모든 sensor 컬럼에 대한 VIF계수를 구하고자 할때 주석해제
     # columns_all = sensor_data.columns
     
     #  일부 컬럼에 대해서  VIF계수를 구하고자 할때 주석해제
     # drop할 컬럼명 넣가
     # drop_columns = []
     # columns_all = [col for col in columns_all if col not in drop_columns]
     
     
     # 피처마다의 VIF 계수를 출력
     vif = pd.DataFrame()
     vif["VIF Factor"] = [variance_inflation_factor(fact_data[columns_all].values, i) for i in range(fact_data[columns_all].values.shape[1])]
     vif["features"] = fact_data[columns_all].columns
     vif
     
     ## 분산팽창 파일 저장
     vifdf = vif.sort_values(by="VIF Factor",ascending=False).round(1)
     vifdf.to_csv(working_dir + 'vifdf.csv')
     ```

   - 파일 실행하기 
   
     - VIF계수를 구하는 처리에서 40분정도 소요 됩니다. 파일을 저장한 후에 불러와서 사용하세요
