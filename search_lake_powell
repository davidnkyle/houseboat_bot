from selenium import webdriver
import smtplib, ssl
# from webdriver_manager.chrome import ChromeDriverManager
import time
# we import the Twilio client from the dependency we just installed
from twilio.rest import Client

# the following line needs your Twilio Account SID and Auth Token
client = Client("account_sid", "auth_token")

# chromeOptions = webdriver.ChromeOptions()
driver = webdriver.Chrome('./chromedriver')

def get_dates(starting_point, month, day):
    driver.get(r'https://reservations.ahlsmsworld.com/LakePowell')
    time.sleep(2)
    driver.find_element_by_name(r'InitialProductSelection').send_keys(starting_point)
    driver.find_element_by_name(r'ArrivalMonthVisible').send_keys('{}, 2020\n\t{}\t\t\n'.format(month, day))
    # driver.find_element_by_name(r'TripLength').send_keys('{}\t\t\n'.format(day))
    all_dates = driver.find_elements_by_class_name('ui-datepickerAvail-significant')
    return all_dates

found_any = False
earliest_date = 31
starting_point_str = 'unknown'

for starting_point in ['Bullfrog Marina Houseboats', 'Wahweap Marina Houseboats']:
    for day in range(5, 8):
        all_dates = get_dates(starting_point, 'Aug', day)
        for date_element in all_dates:
            new_date = int(date_element.text)
            if int(date_element.text) <= 8:
                found_any = True
                if new_date < earliest_date:
                    earliest_date = new_date
                    starting_point_str = starting_point
    earliest_date_str = 'Aug {}'.format(earliest_date)
    for day in range(5, 8):
        all_dates = get_dates(starting_point, 'Jul', day)
        for date_element in all_dates:
            new_date = int(date_element.text)
            if int(date_element.text) == 31:
                found_any = True
                earliest_date_str = 'Jul 31'
                starting_point_str = starting_point


if found_any:
    message = 'Lake Powell Reservation Available for as early as {} for at least 5 nights\n' \
              'leaving from {}\n' \
              'Check it out at https://reservations.ahlsmsworld.com/LakePowell'.format(earliest_date_str, starting_point_str)
else:
    message = 'Unfortunately, no reservations found'

port = 465  # For SSL
password = "----------"

# Create a secure SSL context
context = ssl.create_default_context()

with smtplib.SMTP_SSL("smtp.gmail.com", port, context=context) as server:
    server.login("name@email.com", password)
    receiver_email = 'name@email.com'
    sender_email = 'name@email.com'
    server.sendmail(sender_email, receiver_email, message)
    if found_any:
        receiver_email = 'name@email.com'
        server.sendmail(sender_email, receiver_email, message)
        receiver_email = 'anothername@email.com'
        server.sendmail(sender_email, receiver_email, message)
        receiver_email = 'thirdname@email.com'
        server.sendmail(sender_email, receiver_email, message)
        client.messages.create(to="+11111111111",
                               from_="+11111111111",
                               body=message)
        client.messages.create(to="+11111111111",
                               from_="+11111111111",
                               body=message)
        client.messages.create(to="+11111111111",
                               from_="+11111111111",
                               body=message)
