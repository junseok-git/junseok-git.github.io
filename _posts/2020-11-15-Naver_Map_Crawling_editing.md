# 셀레니움 활용 네이버 지도 크롤링 작업

## 기본 세팅 및 OD 데이터 입력


```python
!pip install selenium
!pip install pandas
!pip install requests
!pip install urllib
!pip install time
```

    Requirement already satisfied: selenium in c:\users\smartmobility2\anaconda3\lib\site-packages (3.141.0)
    Requirement already satisfied: urllib3 in c:\users\smartmobility2\anaconda3\lib\site-packages (from selenium) (1.25.9)
    Requirement already satisfied: pandas in c:\users\smartmobility2\anaconda3\lib\site-packages (1.0.5)
    Requirement already satisfied: python-dateutil>=2.6.1 in c:\users\smartmobility2\anaconda3\lib\site-packages (from pandas) (2.8.1)
    Requirement already satisfied: pytz>=2017.2 in c:\users\smartmobility2\anaconda3\lib\site-packages (from pandas) (2020.1)
    Requirement already satisfied: numpy>=1.13.3 in c:\users\smartmobility2\anaconda3\lib\site-packages (from pandas) (1.18.5)
    Requirement already satisfied: six>=1.5 in c:\users\smartmobility2\anaconda3\lib\site-packages (from python-dateutil>=2.6.1->pandas) (1.15.0)
    Requirement already satisfied: requests in c:\users\smartmobility2\anaconda3\lib\site-packages (2.24.0)
    Requirement already satisfied: idna<3,>=2.5 in c:\users\smartmobility2\anaconda3\lib\site-packages (from requests) (2.10)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\smartmobility2\anaconda3\lib\site-packages (from requests) (2020.6.20)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in c:\users\smartmobility2\anaconda3\lib\site-packages (from requests) (1.25.9)
    Requirement already satisfied: chardet<4,>=3.0.2 in c:\users\smartmobility2\anaconda3\lib\site-packages (from requests) (3.0.4)
    

    ERROR: Could not find a version that satisfies the requirement urllib (from versions: none)
    ERROR: No matching distribution found for urllib
    ERROR: Could not find a version that satisfies the requirement time (from versions: none)
    ERROR: No matching distribution found for time
    


```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup
import pandas as pd
import requests
import urllib
import time
import pandas as pd
import requests
from urllib.request import urlopen
from urllib.parse import quote
import urllib
import re
char =re.compile('[^ 0-9a-zA-Zㄱ-ㅣ가-힣!?#]')

```


```python
path="C:/Users/smartmobility2/Desktop/김준석/IoT_naver/"
data=pd.read_csv(path+"data_iot_6_10_n.csv", encoding='euc-kr')
```


```python
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>id</th>
      <th>trip</th>
      <th>location</th>
      <th>lat</th>
      <th>lng</th>
      <th>id_trip</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.590000e+14</td>
      <td>6</td>
      <td>20080401</td>
      <td>서울특별시 동작구 상도동 421</td>
      <td>37.497949</td>
      <td>126.958316</td>
      <td>06_20080401</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.590000e+14</td>
      <td>6</td>
      <td>20080401</td>
      <td>서울특별시 금천구 가산동 459-9</td>
      <td>37.482043</td>
      <td>126.879537</td>
      <td>06_20080401</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.590000e+14</td>
      <td>6</td>
      <td>20080501</td>
      <td>서울특별시 동작구 상도동 421</td>
      <td>37.497949</td>
      <td>126.958316</td>
      <td>06_20080501</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.590000e+14</td>
      <td>6</td>
      <td>20080501</td>
      <td>서울특별시 금천구 가산동 459-9</td>
      <td>37.482043</td>
      <td>126.879537</td>
      <td>06_20080501</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3.590000e+14</td>
      <td>6</td>
      <td>20080502</td>
      <td>서울특별시 금천구 가산동 459-9</td>
      <td>37.482043</td>
      <td>126.879537</td>
      <td>06_20080502</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>306</th>
      <td>3.590000e+14</td>
      <td>10</td>
      <td>20081801</td>
      <td>가산디지털단지역</td>
      <td>37.481465</td>
      <td>126.882650</td>
      <td>10_20081801</td>
    </tr>
    <tr>
      <th>307</th>
      <td>3.590000e+14</td>
      <td>10</td>
      <td>20081802</td>
      <td>가산디지털단지역</td>
      <td>37.481465</td>
      <td>126.882650</td>
      <td>10_20081802</td>
    </tr>
    <tr>
      <th>308</th>
      <td>3.590000e+14</td>
      <td>10</td>
      <td>20081802</td>
      <td>대림역</td>
      <td>37.492504</td>
      <td>126.894961</td>
      <td>10_20081802</td>
    </tr>
    <tr>
      <th>309</th>
      <td>3.590000e+14</td>
      <td>10</td>
      <td>20081802</td>
      <td>강남역</td>
      <td>37.497950</td>
      <td>127.027637</td>
      <td>10_20081802</td>
    </tr>
    <tr>
      <th>310</th>
      <td>3.590000e+14</td>
      <td>10</td>
      <td>20081802</td>
      <td>청계산입구역</td>
      <td>37.448224</td>
      <td>127.054738</td>
      <td>10_20081802</td>
    </tr>
  </tbody>
</table>
<p>311 rows × 7 columns</p>
</div>




```python
id_lst=list(set(data["id_trip"]))
id_lst.sort()

start_lat=[]
end_lat=[]

start_lat.append(data['location'][0])

for i in range(1,len(data)-1):
    if data['id_trip'][i-1]!=data['id_trip'][i]:
        start_lat.append(data['location'][i])
    elif data['id_trip'][i]!=data['id_trip'][i+1]:
        end_lat.append(data['location'][i])

end_lat.append(data['location'][len(data)-1])


print(len(id_lst))
print(len(start_lat))
print(len(end_lat))

id=pd.DataFrame(id_lst)
start_df=pd.DataFrame(start_lat)
end_df=pd.DataFrame(end_lat)

df=pd.concat([id, start_df, end_df], axis=1)
df.columns=['id', 'start', 'end']
df=df.drop_duplicates(['start', 'end'], keep='last')
df=df.reset_index(drop=True)
df
```

    109
    109
    109
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>start</th>
      <th>end</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>06_20080502</td>
      <td>서울특별시 금천구 가산동 459-9</td>
      <td>홍대입구</td>
    </tr>
    <tr>
      <th>1</th>
      <td>06_20080503</td>
      <td>홍대입구</td>
      <td>서울특별시 동작구 상도동 421</td>
    </tr>
    <tr>
      <th>2</th>
      <td>06_20080804</td>
      <td>강남역</td>
      <td>노량진</td>
    </tr>
    <tr>
      <th>3</th>
      <td>06_20081002</td>
      <td>시청역</td>
      <td>서울특별시 동작구 상도동 421</td>
    </tr>
    <tr>
      <th>4</th>
      <td>06_20081102</td>
      <td>서울특별시 금천구 가산동 459-9</td>
      <td>서울특별시 동작구 상도동 421</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>86</th>
      <td>10_20081601</td>
      <td>둔촌동역</td>
      <td>서울시 송파구 마천동 송파파크데일 1단지</td>
    </tr>
    <tr>
      <th>87</th>
      <td>10_20081701</td>
      <td>마천역</td>
      <td>군자역</td>
    </tr>
    <tr>
      <th>88</th>
      <td>10_20081702</td>
      <td>군자역</td>
      <td>마천역</td>
    </tr>
    <tr>
      <th>89</th>
      <td>10_20081801</td>
      <td>오금역</td>
      <td>가산디지털단지역</td>
    </tr>
    <tr>
      <th>90</th>
      <td>10_20081802</td>
      <td>가산디지털단지역</td>
      <td>청계산입구역</td>
    </tr>
  </tbody>
</table>
<p>91 rows × 3 columns</p>
</div>



## 크롤링 실행. Delay Time 수정 적용중..


```python
driver=webdriver.Chrome("C:/Users/smartmobility2/Desktop/김준석/IoT_naver/chromedriver.exe")

```


```python
delay_time=60
```


```python
for num in range(5,len(df)):
    driver.get("https://map.naver.com/v5/directions/-/-/-/transit?c=14142425.8831862,4519981.4735816,15,0,0,0,dh")
    driver.implicitly_wait(delay_time)


    start=df['start'][num]
    end=df['end'][num]

    #출발지와 도착지 입력

    driver.find_element_by_id("directionStart0").send_keys(start)
    driver.implicitly_wait(delay_time)
    #driver.find_element_by_id("directionStart0").send_keys(Keys.ENTER)
    driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.main > div.search_area > directions-search > div.search_box > directions-search-box.directions-search-item-el.droppable.item_search.start > div > directions-search-box-instant-search > div > instant-search-list > div > div > nm-list-container:nth-child(2) > div > nm-list > ul > li > a > div").click()

    driver.implicitly_wait(delay_time)

    driver.find_element_by_id("directionGoal1").send_keys(end)
    driver.implicitly_wait(delay_time)
    #driver.find_element_by_id("directionGoal1").send_keys(Keys.ENTER)

    driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.main > div.search_area > directions-search > div.search_box > directions-search-box:nth-child(2) > div > directions-search-box-instant-search > div > instant-search-list > div > div > nm-list-container:nth-child(2) > div > nm-list > ul > li:nth-child(1) > a > div").click()
    #driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.main > div > directions-search > div.btn_box > button.btn.btn_direction.active").click()

    driver.implicitly_wait(delay_time)

    #길찾기 버튼 클릭
    driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.main > div > directions-search > div.btn_box > button.btn.btn_direction.active").click()
    
    #최적경로 클릭
    driver.implicitly_wait(delay_time)
    driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.main > directions-summary-list > directions-hover-scroll > div > div > directions-summary-item-pubtransit.item_summary.selected.ng-star-inserted > div:nth-child(1)").click()

    #세부경로 가져오기
    driver.implicitly_wait(delay_time)
    k=driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted > div")
    k=k.text.split("\n")
    driver.implicitly_wait(delay_time)

    count=0
    for s in k:
        if "승차" in s:
            count+=1
        
    for i in range(1,(count+1)*2): 
        css="#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted > div > ul > li:nth-child("+str(i)+") > directions-detail-guide-pubtransit > div:nth-child(1) > div > button"
        #print(css)
        try:
            driver.find_element_by_css_selector(css).click()
        except:
            pass
        
        
    k=driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted > div")
    k=k.text.split("\n") 
    def remove_values_from_list(the_list, val):
       return [value for value in the_list if value != val]
    k=remove_values_from_list(k, "파노라마 보기")
    k=remove_values_from_list(k, "도보")
    k=remove_values_from_list(k, "상세정보")
    k=remove_values_from_list(k, "거리뷰")
    k=remove_values_from_list(k, "운행정보")

    matching = [s for s in k if "분" in s] 
    for i in matching:
        k=remove_values_from_list(k, i)
    
    matching = [s for s in k if "-" in s] 
    for i in matching:
        k=remove_values_from_list(k, i)
        
    matching = [s for s in k if "지선" in s] 
    for i in matching:
        k=remove_values_from_list(k, i)
        
    matching = [s for s in k if "도보" in s] 
    for i in matching:
        k=remove_values_from_list(k, i)
        
    matching = [s for s in k if "이동" in s] 
    for i in matching:
        k=remove_values_from_list(k, i)

    del(k[-1])

    
    dfk=pd.DataFrame(k)
    dfk.columns=[df['id'][num]]

    
    f_name=df['id'][num]+'.csv'
    dfk.to_csv(path+'output/'+f_name, encoding='euc-kr')
```


    ---------------------------------------------------------------------------

    NoSuchElementException                    Traceback (most recent call last)

    <ipython-input-34-f9097b702ac0> in <module>
         34     #세부경로 가져오기
         35     driver.implicitly_wait(delay_time)
    ---> 36     k=driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted > div")
         37     k=k.text.split("\n")
         38     driver.implicitly_wait(delay_time)
    

    ~\anaconda3\lib\site-packages\selenium\webdriver\remote\webdriver.py in find_element_by_css_selector(self, css_selector)
        596             element = driver.find_element_by_css_selector('#foo')
        597         """
    --> 598         return self.find_element(by=By.CSS_SELECTOR, value=css_selector)
        599 
        600     def find_elements_by_css_selector(self, css_selector):
    

    ~\anaconda3\lib\site-packages\selenium\webdriver\remote\webdriver.py in find_element(self, by, value)
        974                 by = By.CSS_SELECTOR
        975                 value = '[name="%s"]' % value
    --> 976         return self.execute(Command.FIND_ELEMENT, {
        977             'using': by,
        978             'value': value})['value']
    

    ~\anaconda3\lib\site-packages\selenium\webdriver\remote\webdriver.py in execute(self, driver_command, params)
        319         response = self.command_executor.execute(driver_command, params)
        320         if response:
    --> 321             self.error_handler.check_response(response)
        322             response['value'] = self._unwrap_value(
        323                 response.get('value', None))
    

    ~\anaconda3\lib\site-packages\selenium\webdriver\remote\errorhandler.py in check_response(self, response)
        240                 alert_text = value['alert'].get('text')
        241             raise exception_class(message, screen, stacktrace, alert_text)
    --> 242         raise exception_class(message, screen, stacktrace)
        243 
        244     def _value_or_default(self, obj, key, default):
    

    NoSuchElementException: Message: no such element: Unable to locate element: {"method":"css selector","selector":"#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted > div"}
      (Session info: chrome=87.0.4280.66)
    


### ↑ 주기적 오류 발생, Delay Time 연장시켜 보는중..


```python
start=df['start'][i]
#start=start.replace("'","")
start
```




    '서울특별시 도봉구 창동 804'




```python
start
```




    '서울특별시 도봉구 창동 804'




```python
end

```




    '아이파크몰'



## 좌표로 OD 설정하고 크롤링 실행해보기


```python
id_lst=list(set(data["id_trip"]))
id_lst.sort()

start_lat=[]
start_lng=[]
end_lat=[]
end_lng=[]

start_lat.append(data['lat'][0])
start_lng.append(data['lng'][0])

for i in range(1,len(data)-1):
    if data['id_trip'][i-1]!=data['id_trip'][i]:
        start_lat.append(data['lat'][i])
        start_lng.append(data['lng'][i])
    elif data['id_trip'][i]!=data['id_trip'][i+1]:
        end_lat.append(data['lat'][i])
        end_lng.append(data['lng'][i])

end_lat.append(data['lat'][len(data)-1])
end_lng.append(data['lng'][len(data)-1])


print(len(id_lst))
print(len(start_lat))
print(len(end_lng))

id=pd.DataFrame(id_lst)
start_lat=pd.DataFrame(start_lat)
start_lng=pd.DataFrame(start_lng)
end_lat=pd.DataFrame(end_lat)
end_lng=pd.DataFrame(end_lng)

df=pd.concat([id, start_lat, start_lng, end_lat, end_lng], axis=1)
df.columns=['id', 'start_lat', 'start_lng', 'end_lat', 'end_lng']
df
```

    103
    103
    103
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>start_lat</th>
      <th>start_lng</th>
      <th>end_lat</th>
      <th>end_lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1_20081101</td>
      <td>37.642632</td>
      <td>127.037807</td>
      <td>37.529443</td>
      <td>126.964332</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1_20081102</td>
      <td>37.529443</td>
      <td>126.964332</td>
      <td>37.642632</td>
      <td>127.037807</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1_20081201</td>
      <td>37.642632</td>
      <td>127.037807</td>
      <td>37.639777</td>
      <td>127.025518</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1_20081202</td>
      <td>37.639777</td>
      <td>127.025518</td>
      <td>37.642632</td>
      <td>127.037807</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1_20081301</td>
      <td>37.642632</td>
      <td>127.037807</td>
      <td>37.535152</td>
      <td>127.094689</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>98</th>
      <td>5_20081602</td>
      <td>37.530165</td>
      <td>126.853367</td>
      <td>37.529328</td>
      <td>126.845996</td>
    </tr>
    <tr>
      <th>99</th>
      <td>5_20081603</td>
      <td>37.529328</td>
      <td>126.845996</td>
      <td>37.537778</td>
      <td>126.836695</td>
    </tr>
    <tr>
      <th>100</th>
      <td>5_20081604</td>
      <td>37.537778</td>
      <td>126.836695</td>
      <td>37.529328</td>
      <td>126.845996</td>
    </tr>
    <tr>
      <th>101</th>
      <td>5_20081701</td>
      <td>37.529328</td>
      <td>126.845996</td>
      <td>37.554158</td>
      <td>126.970441</td>
    </tr>
    <tr>
      <th>102</th>
      <td>5_20081702</td>
      <td>37.554158</td>
      <td>126.970441</td>
      <td>37.529328</td>
      <td>126.845996</td>
    </tr>
  </tbody>
</table>
<p>103 rows × 5 columns</p>
</div>




```python
driver=webdriver.Chrome("C:/Users/smartmobility2/Desktop/김준석/IoT_naver/chromedriver.exe")
delay_time=5

```


```python
driver.get("https://map.naver.com/v5/directions/-/-/-/transit?c=14142425.8831862,4519981.4735816,15,0,0,0,dh")

```


```python
start=data['xy'][0]
end=data['xy'][3]
```


```python
#출발지와 도착지 입력

driver.find_element_by_id("directionStart0").send_keys(start)
driver.implicitly_wait(delay_time)
driver.find_element_by_id("directionStart0").send_keys(Keys.ENTER)

#driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.main > div.search_area > directions-search > div.search_box > directions-search-box.directions-search-item-el.droppable.item_search.start > div > directions-search-box-instant-search > div > instant-search-list > div > div > nm-list-container:nth-child(2) > div > nm-list > ul > li:nth-child(2)").click()
driver.find_element_by_id("directionGoal1").send_keys(end)
driver.find_element_by_id("directionGoal1").send_keys(Keys.ENTER)

driver.implicitly_wait(delay_time)
#driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.main > div.search_area > directions-search > div.search_box > directions-search-box:nth-child(2) > div > directions-search-box-instant-search > div > instant-search-list > div > div > nm-list-container:nth-child(2) > div > nm-list > ul > li:nth-child(1)").click()

```


```python
#최적경로 클릭
driver.implicitly_wait(delay_time)

driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.main > directions-summary-list > directions-hover-scroll > div > div > directions-summary-item-pubtransit.item_summary.selected.ng-star-inserted > div:nth-child(1)").click()

```


```python
#세부경로 가져오기
k=driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted > div")
#k=driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted").text
#k.split("\n")
#driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted > div").text
k=k.text.split("\n")

```


```python
count=0
for s in k:
    if "승차" in s:
        count+=1
        
for i in range(1,(count+1)*2): 
    css="#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted > div > ul > li:nth-child("+str(i)+") > directions-detail-guide-pubtransit > div:nth-child(1) > div > button"
    #print(css)
    try:
        driver.find_element_by_css_selector(css).click()
    except:
        pass
```


```python
k=driver.find_element_by_css_selector("#container > shrinkable-layout > div > directions-layout > directions-result > div.sub.-covered-top0 > directions-details-result > directions-hover-scroll > div > div.public_result_area.ng-star-inserted > div")
k=k.text.split("\n") 
def remove_values_from_list(the_list, val):
   return [value for value in the_list if value != val]
k=remove_values_from_list(k, "파노라마 보기")
k=remove_values_from_list(k, "도보")
k=remove_values_from_list(k, "상세정보")
k=remove_values_from_list(k, "거리뷰")
k=remove_values_from_list(k, "운행정보")

matching = [s for s in k if "분" in s] 
for i in matching:
    k=remove_values_from_list(k, i)

matching = [s for s in k if "-" in s] 
for i in matching:
    k=remove_values_from_list(k, i)
    
matching = [s for s in k if "지선" in s] 
for i in matching:
    k=remove_values_from_list(k, i)
    
matching = [s for s in k if "도보" in s] 
for i in matching:
    k=remove_values_from_list(k, i)
    
matching = [s for s in k if "이동" in s] 
for i in matching:
    k=remove_values_from_list(k, i)

del(k[-1])

k
pd.DataFrame(k)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>출발</td>
    </tr>
    <tr>
      <th>1</th>
      <td>37°38'33"N 127°02'16"E</td>
    </tr>
    <tr>
      <th>2</th>
      <td>버스</td>
    </tr>
    <tr>
      <th>3</th>
      <td>대우아파트104동 승차</td>
    </tr>
    <tr>
      <th>4</th>
      <td>창동대우아파트</td>
    </tr>
    <tr>
      <th>5</th>
      <td>창2동주민센터·보건지소</td>
    </tr>
    <tr>
      <th>6</th>
      <td>창2동주민센터.보건지소</td>
    </tr>
    <tr>
      <th>7</th>
      <td>태영데시앙아파트</td>
    </tr>
    <tr>
      <th>8</th>
      <td>우이1교앞</td>
    </tr>
    <tr>
      <th>9</th>
      <td>쌍문역 하차</td>
    </tr>
    <tr>
      <th>10</th>
      <td>지하철</td>
    </tr>
    <tr>
      <th>11</th>
      <td>4호선 쌍문역 승차</td>
    </tr>
    <tr>
      <th>12</th>
      <td>수유(강북구청)역 방면(오이도행)</td>
    </tr>
    <tr>
      <th>13</th>
      <td>수유(강북구청)</td>
    </tr>
    <tr>
      <th>14</th>
      <td>미아</td>
    </tr>
    <tr>
      <th>15</th>
      <td>미아사거리</td>
    </tr>
    <tr>
      <th>16</th>
      <td>길음</td>
    </tr>
    <tr>
      <th>17</th>
      <td>성신여대입구</td>
    </tr>
    <tr>
      <th>18</th>
      <td>한성대입구</td>
    </tr>
    <tr>
      <th>19</th>
      <td>혜화</td>
    </tr>
    <tr>
      <th>20</th>
      <td>동대문</td>
    </tr>
    <tr>
      <th>21</th>
      <td>동대문역사문화공원</td>
    </tr>
    <tr>
      <th>22</th>
      <td>충무로</td>
    </tr>
    <tr>
      <th>23</th>
      <td>명동</td>
    </tr>
    <tr>
      <th>24</th>
      <td>회현</td>
    </tr>
    <tr>
      <th>25</th>
      <td>서울역</td>
    </tr>
    <tr>
      <th>26</th>
      <td>숙대입구</td>
    </tr>
    <tr>
      <th>27</th>
      <td>삼각지</td>
    </tr>
    <tr>
      <th>28</th>
      <td>신용산역 하차</td>
    </tr>
    <tr>
      <th>29</th>
      <td>도착</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.to_csv(path+'output/data.csv', encoding='euc-kr')
```
