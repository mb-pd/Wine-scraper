# Wine-scraper
import requests

url = "https://www.burrowingowlwine.ca/wp-admin/admin-ajax.php"

# Playload 
payload = {
    "action": "filter_wines",
    "title": "Chardonnay",  #Change this to the desired wine type
    "vintage": "2013"  # Change this to the desired year
}

# Headers
headers = {
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)",
    "Accept": "application/json, text/javascript, */*; q=0.01",
    "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
    "X-Requested-With": "XMLHttpRequest",
    "Origin": "https://www.burrowingowlwine.ca",
    "Referer": "https://www.burrowingowlwine.ca/wine/"
}

# Send request
response = requests.post(url, data=payload, headers=headers)

# Check if request was successful
if response.status_code == 200:
    try:
        data = response.json() 
        print("Response:", data) 

        # Extract the Featured Sheet PDF URL
        if data.get("success") and data.get("data"):
            wine_data = data["data"][0]
            featured_sheet_url = wine_data["files"]["_meta_files_pdf"]["url"]
            print(f"Download URL: {featured_sheet_url}")
        else:
            print("No wine data found.")

    except ValueError:
        print("Failed to parse JSON response:", response.text)

else:
    print(f"Request failed with status: {response.status_code}")
