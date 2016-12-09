region_dummy = pd.get_dummies(house_price_final_region['region'])
house_price_dummy = pd.concat([house_price_final_region, region_dummy],axis=1)
