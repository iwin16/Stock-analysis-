# Stock-analysis-
Positivity, Negativity, All time high, All time low
# add a .txt file with names of scrips (as in yahoo finace, forexample Tatamotors - "TATAMOTORS.NS", in capital letters with code name of exchange) line by line(with any numbering). yfinance library is used to retrieve data.

import pandas as pd
import yfinance as yf

#scriptionary
scrip_list = open("C:/scriplist.txt", 'r')
with open("C:/Users/IwinJoseph/SM/scriplist.txt", 'r') as f:
    scrip_list = [line.rstrip('\n') for line in f]
     

start_date = '2016-01-01'
end_date = '2021-12-08'

for scrip in scrip_list:
    scrip_df =yf.download(scrip, 
              start=start_date,
              end=end_date,
              progress=False) 
    scriptionary[scrip] = scrip_df
    

    #for adding change
    
    
    vals = []
    ncolumn =  scriptionary[scrip].shape[0]-1
    jsaw = scriptionary[scrip]
    
    for i in range(ncolumn):
        val1 = jsaw.iloc[i,4]

        val2 = jsaw.iloc[i+1,4]
        val = ((val1-val2)/val2)*100

        percent = round(val,2)

        vals.append(percent) # append value to list 'vals'
    vals.insert(0,0)

    jsaw['Change'] = vals

    scriptionary[scrip].append(jsaw)
    

    
    

    # calculating atl,ath, positivity and negativity
    positive = []
    negative = []
    zero = []
    for i in range(ncolumn):

        if (jsaw.iloc[i,6]) >0:
            positive.append(jsaw.iloc[i])
        elif (jsaw.iloc[i,6])< 0:
            negative.append(jsaw.iloc[i])
        elif (jsaw.iloc[i,6]) == 0:
            zero.append(jsaw.iloc[i])
  
        

    positivity = (len(positive)/(ncolumn+1))*100
    negativity = (len(negative)/(ncolumn+1))*100
    rp = round(positivity,2)
    rn = round(negativity,2)
    ath1 = jsaw["Close"].max()
    atl1 = jsaw["Close"].min()
    ath = round(ath1,2)
    atl = round(atl1,2)


    print("This is the data of " + scrip + " from the past 5 years.")
    print('Positivity =',str(rp)  + "\n" +
          "Negativity = ",str(rn) +"\n" +  "All time high =",str(ath) +
          "\n"+ "All time low =", str(atl) + "\n")
