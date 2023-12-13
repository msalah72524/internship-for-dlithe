# internship-for-dlithe
# The project forecasting price of the crops.
# The project is based on the prediction of price of the commodity using machine learning
import streamlit as stt
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestRegressor
import requests
import datetime
from streamlit_lottie import st_lottie
def stream(list1,list2,list3,list4,mon,sea,year,day):
    def load_lottie(url):
        r=requests.get(url)
        if r.status_code !=200:
            return None
        return r.json()
    lottie_coding2=load_lottie("https://lottie.host/e8bccb20-d530-4e03-82b2-3c9b671832ef/XvGsCfko6g.json")
    with stt.container():
        leftc,rightc=stt.columns(2)
        with leftc:
            stt.header("Forecasting price for crops")
            stt.write("Predict the best value for the crops")
        with rightc:
                st_lottie(lottie_coding2,height=300,key="code")  
    def load_lottie(url):
        r=requests.get(url)
        if r.status_code !=200:
            return None
        return r.json()
    lottie_coding=load_lottie("https://lottie.host/c794bb31-62b9-4c71-b6f2-734996eb9d98/37Z9dLVvmh.json")
    option1=list(range(len(list1)))
    option2=list(range(len(list2)))
    option3=list(range(len(list3)))
    option4=list(range(len(list4)))
    option6=list(range(len(sea)))
    with stt.container():
        stt.write("---")
        leftc,rightc=stt.columns(2)
        with leftc:
            stt.write("Get the best price for ur crops at ur location" )
            stt.write( "Predict the maximum value for the ur crops at the specific date and season")
            stt.write("The price predication of the commodity is based on  per 50kg")
        with rightc:
            st_lottie(lottie_coding,height=200,key="coding")    

    with stt.container():
        stt.write("enter the state")
        sta1=stt.selectbox("",option1,format_func=lambda x:list1[x])
        stt.write("enter the district")
        dis1=stt.selectbox("",option2,format_func=lambda x:list2[x])
        stt.write("enter the market")
        mar1=stt.selectbox("",option3,format_func=lambda x:list3[x])
        stt.write("enter the commodity")
        com1=stt.selectbox("",option4,format_func=lambda x:list4[x])
        stt.write("enter the day")
        day1=stt.selectbox("",day)
        int(day1)
        mon1=1
        stt.write("enter the month")
        mo=stt.selectbox("",mon)
        moonth={"jan":1,"feb":2,"march":3,"may":4,"april":5,"june":6,"july":7,"aug":8,"sept":9,"oct":10,"nov":11,"dec":12}
        mon1=moonth[mo]
        stt.write("enter the year")
        year1=stt.selectbox("",year)
        int(year1)
        stt.write("enter the season")
        sea1=stt.selectbox("",option6,format_func=lambda x:sea[x])
        laa=datetime.datetime(year1,mon1,day1)
        week=laa.strftime('%A')
        saas={'Sunday':1,'Monday':2,'Tuesday':3,'Wednesday':4,'Thrusday':5,'Friday':6,'Saturday':7}
        week_day=saas[week]
    return sta1,dis1,mar1,com1,mon1,sea1,week_day
   
   
def stream_print(x):
    with stt.container():
        stt.write("The estimated price for ur commodity per 50kg is Rs",x)

def main():

    file=pd.read_csv("test2.csv")
    d3=pd.DataFrame(file) 
    df=d3.drop(["variety","grade","Min Price","Max Price"],axis=1)
    df.isnull().sum()
    data2=df.copy()
    dict={1:'jan',2:'feb',3:'march',4:'april',5:'may',6:'june',7:'july',8:'august',9:'september',10:'october',11:'november',12:'december'}
    month_column=[]
    for i in data2['Arrival_Date']:
        str=i
        str2=str.split('-')
        month_column.append(dict[(int(str2[1]))])
    data2["month_column"]=month_column
    season_name=[]
    for t in data2["month_column"]:
        if t=='november' or t=='feb'or t=='jan' or t=='december':
            season_name.append('winter')
        elif t=='march' or t=='may'or  t=='april':
            season_name.append('summer')
        elif t=="august" or t=="june"or t=="july" or t=="october"or t=="september":
            season_name.append('rainy')
    data2["season_name"]=season_name 
    day_week=[]    
    for r in data2["Arrival_Date"]:
        str=r
        de=pd.Timestamp(r)
        day=de.dayofweek
        day_week.append(day)
    len(day_week)
    data2["day"]=day_week
    data2=data2.drop('Arrival_Date',axis=1)
    data2=data2.head(23000)
    Q1=np.percentile(data2['Modal Price'],25,interpolation ="midpoint")
    Q3=np.percentile(data2['Modal Price'],75,interpolation ="midpoint")
    IQR=Q3-Q1  
    upper = np.where(data2['Modal Price'] >= (Q3+1.5*IQR))
    lower = np.where(data2['Modal Price']<= (Q1-1.5*IQR))
    data2.drop(upper[0],inplace=True)
    data2.drop(lower[0],inplace=True)
    df=data2.copy()
    list1=[""]
    list2=[""]
    list3=[""]
    list4=[""]
    a=df['State'].unique()
    for i in range (len(a)):
        list1.append(a[i])            
    b=df['District'].unique()
    for i in range (len(b)):
        list2.append(b[i])           
    c=df['Market'].unique()
    for i in range (len(c)):
        list3.append(c[i])            
    d=df['Commodity'].unique()
    for i in range (len(d)):
        list4.append(d[i])        
    list5=["jan","feb","march","may","april","june","july","aug","sept","oct","nov","dec"]
    list6=["","summer","winter","rainy"]
    list7=[ 2023, 2024, 2025, 2026, 2027, 2028, 2029, 2030, 2031, 2032, 2033, 2034, 2035, 2036, 2037, 2038, 2039, 2040, 2041, 2042, 2043, 2044, 2045, 2046, 2047, 2048, 2049, 2050, 2051, 2052, 2053, 2054, 2055, 2056, 2057, 2058, 2059, 2060, 2061, 2062, 2063, 2064, 2065, 2066, 2067, 2068, 2069, 2070, 2071, 2072, 2073, 2074, 2075, 2076, 2077, 2078, 2079, 2080, 2081, 2082, 2083, 2084, 2085, 2086, 2087, 2088, 2089, 2090, 2091, 2092, 2093, 2094, 2095, 2096, 2097, 2098]
    list8=[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31]
    dist=(data2['Commodity'])
    distset=set(dist)
    dd=list(distset)
    dictOfwords={dd[i] :i for i in range(0,len(dd))}
    data2['Commodity']=data2['Commodity'].map(dictOfwords)
    dist=(data2['State'])
    distset=set(dist)
    dd=list(distset)
    dictOfwords={dd[i] :i for i in range(0,len(dd))}
    data2['State']=data2['State'].map(dictOfwords)
    dist=(data2['District'])
    distset=set(dist)
    dd=list(distset)
    dictOfwords={dd[i] :i for i in range(0,len(dd))}
    data2['District']=data2['District'].map(dictOfwords)
    dist=(data2['Market'])
    distset=set(dist)
    dd=list(distset)
    dictOfwords={dd[i] :i for i in range(0,len(dd))}
    data2['Market']=data2['Market'].map(dictOfwords)
    dist=(data2['month_column'])
    distset=set(dist)
    dd=list(distset)
    dictOfwords={dd[i] :i for i in range(0,len(dd))}
    data2['month_column']=data2['month_column'].map(dictOfwords)
    dist=(data2['season_name'])
    distset=set(dist)
    dd=list(distset)
    dictOfwords={dd[i] :i for i in range(0,len(dd))}
    data2['season_name']=data2['season_name'].map(dictOfwords)
    features=data2[['State', 'District', 'Market', 'Commodity',
            'month_column', 'season_name', 'day']]
    labels=data2['Modal Price']
    Xtrain,Xtest,Ytrain,Ytest=train_test_split(features,labels,test_size=0.2,random_state=2)
    regr=RandomForestRegressor(max_depth=1000,random_state=50)
    regr.fit(Xtrain,Ytrain)
    y_pred=regr.predict(Xtest)
    from sklearn.metrics import r2_score
    r2_score(Ytest,y_pred)
    st,di,ma,co,mo,se,da=stream(list1,list2,list3,list4,list5,list6,list7,list8)
    test=[]
    test.append(st)
    test.append(di)
    test.append(ma)
    test.append(co)
    test.append(mo)
    test.append(se)
    test.append(da)
    user_input=[]
    user_input.append(test)
    x=regr.predict(user_input)
    result=stt.button("Predict")
    if result==True:
          stream_print(x)
if __name__ == "__main__":
        stt.set_page_config(page_title="Forecasting price for crops",layout="wide")
        main()
