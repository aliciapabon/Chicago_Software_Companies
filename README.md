# Chicago_Software_Companies

from bs4 import BeautifulSoup
import requests
import pandas as pd

#create lists which contain companies information
company_name_list, company_detail_list, company_rating_list, company_pricing_list, company_employees_list, company_founded_list, company_more_info_list = ([] for i in range(7))

#The website has three pages that's why the cycle is three
for i in range(1,4):
    html_ = 'https://www.goodfirms.co/directory/city/top-software-development-companies/chicago?page='
    html_text = requests.get(html_+str(i)).text
    soup = BeautifulSoup(html_text, 'lxml')
    #this class contain all page's companies
    companies = soup.find_all('div', class_ = 'service-entity')

    # extract main data of the company
    for company in companies:
    #There are sections of the page with diferent classes that's why it's necessary use try-except
        try:
            company_name = company.find('a', class_='c_name_head visit-website list-service-website-visit').text
            company_more_info = company.find('a', class_='visit-website linkbtn list-service-website-visit old-btn')[
                'href']
            company_detail = company.find('span', class_='profile-tagline').text
            company_rating = company.find('span', class_='listinv_review_label').text
            company_pricing = company.find('div', class_='firm-pricing').text
            company_employees = company.find('div', class_='firm-employees').text
            company_founded = company.find('div', class_='firm-founded').text
            #Each new data is adding to the existing list using .append
            company_name_list.append(company_name)
            company_detail_list.append(company_detail)
            company_rating_list.append(company_rating)
            company_pricing_list.append(company_pricing)
            company_employees_list.append(company_employees)
            company_founded_list.append(company_founded)
            company_more_info_list.append(company_more_info)
        except:
            pass
    for company in companies:
            try:
                company_name = company.find('a', class_ = 'c_name_head detail_page list-service-profile-visit').text
                #c_name_head detail_page list - service - profile - visit
                company_more_info = company.find('a', class_ = 'visit-website linkbtn list-service-website-visit old-btn')['href']
                company_detail = company.find('span', class_='profile-tagline').text
                company_rating = company.find('span', class_ = 'listinv_review_label').text
                company_pricing = company.find('div', class_='firm-pricing').text
                company_employees = company.find('div', class_='firm-employees').text
                company_founded = company.find('div', class_='firm-founded').text
                company_name_list.append(company_name)
                company_detail_list.append(company_detail)
                company_rating_list.append(company_rating)
                company_pricing_list.append(company_pricing)
                company_employees_list.append(company_employees)
                company_founded_list.append(company_founded)
                company_more_info_list.append(company_more_info)
            except:
                pass

#Create a DataFrame using pandas package
df = pd.DataFrame({'Companies':company_name_list, 'Detail':company_detail_list, 'Rating':company_rating_list, 'Pricing':company_pricing_list, 'Employees':company_employees_list, 'Founded':company_founded_list, 'URL' :company_more_info_list})
#Export the table on a CSV file
df.to_csv('companies_chicago2.csv',  index=False, encoding='UTF-8')
