import pandas as pd
import os
#set directory
path="/Users/guldemirtas/Dropbox/Codecademy/data"
os.chdir(path)

#import data from excel files
visits = pd.read_excel("visits.xlsx", sheet_name = "Sheet1", parse_dates = [1])
cart = pd.read_excel("cart.xlsx", sheet_name = "Sheet1", parse_dates = [1])
checkout = pd.read_excel("checkout.xlsx", sheet_name = "Sheet1", parse_dates = [1])
purchase = pd.read_excel("purchase.xlsx", sheet_name = "Sheet1", parse_dates = [1])

#inspect data frames
#print(visits.head())
#print(cart.head())
#print(checkout.head())
#print(purchase.head())

#inspect the uniqueness of user_id's in each table
uniqueness = pd.DataFrame({"visits" : [visits.user_id.count(),visits.user_id.nunique()],\
                                                "cart": [cart.user_id.count(), cart.user_id.nunique()],\
                                                "checkout": [checkout.user_id.count(), checkout.user_id.nunique()],\
                                                "purchase": [purchase.user_id.count(), purchase.user_id.nunique()]})
uniqueness = uniqueness[["visits", "cart", "checkout", "purchase"]]
print("The matrix showing the count and unique count of each table:")
print(uniqueness)

#eliminate duplicate user_id's from cart, checkout and purchase tables
cart.drop_duplicates(subset = ["user_id"], keep="first", inplace=True)
checkout.drop_duplicates(subset = ["user_id"], keep="first", inplace=True)
purchase.drop_duplicates(subset = ["user_id"], keep="first", inplace=True)

#question 2: combine visits and cart using a left merge
visits_cart_merged = pd.merge(visits, cart, how = "left")

#question 3: How long is your merged DataFrame?
print("Q3: The length of the visits/cart merge:")
print(visits_cart_merged.shape[0])

#question 4: How many of the timestamps are null for the column cart_time?
print("Q4: Number of timestamps that are null for the column cart_time:")
print(sum(visits_cart_merged.cart_time.isnull()))

#question 5: What percent of users who visited ended up not placing a t-shirt in their cart?
print("Q5: Percent of users who visited the website but did not place a tshirt in their cart:")
print((visits_cart_merged.cart_time.isnull()).mean()*100)

#question 6:
#Merge cart and checkout
cart_checkout_merged = pd.merge(cart, checkout, how = "left")

print("Q6: Number of timestamps that are null for the column checkout_time:")
print(sum(cart_checkout_merged.checkout_time.isnull()))

print("Q7: Percent of users who put items in their cart but did not proceed to checkout:")
print((cart_checkout_merged.checkout_time.isnull()).mean()*100)

#question 7:
all_data = visits.merge(cart, how="left").merge(checkout, how="left").merge(purchase, how="left")
#print(all_data.head())

#question 8: What percentage of users proceeded to checkout, but did not purchase a t-shirt?
#First option: Calculate from all_data
print("Q8 / Option 1 Result: Percent of users proceeded to checkout, but did not purchase a t-shirt")
print(1-float(sum(all_data.purchase_time.notnull()))/sum(all_data.checkout_time.notnull()))

#Second option: Calculate from the merge of checkout and purchase
print("Q8 / Option 2 Result: Percent of users proceeded to checkout, but did not purchase a t-shirt")
checkout_purchase = checkout.merge(purchase, how =  'left')
percent_checkout_no_purchase = checkout_purchase[checkout_purchase.purchase_time.isnull()]\
.shape[0] * 100.0 / checkout_purchase[checkout_purchase.checkout_time.notnull()].shape[0]
print(percent_checkout_no_purchase)
