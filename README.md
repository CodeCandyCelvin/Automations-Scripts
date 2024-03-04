# Automations-Scripts
Automation Scripts which is helpful for you to Automate Zapier.
These Scripts are wirtten by Zapier Experts
https://easyaiz.com/zapier-experts/
import gspread
from oauth2client.service_account import ServiceAccountCredentials
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

# Google Sheets credentials
scope = ['https://spreadsheets.google.com/feeds',
         'https://www.googleapis.com/auth/drive']
creds = ServiceAccountCredentials.from_json_keyfile_name('google_sheets_credentials.json', scope)
client = gspread.authorize(creds)

# Connect to the Google Sheet
sheet = client.open('Your Google Sheet Name').sheet1

# Get data from Google Sheet
data = sheet.get_all_records()

# Define email parameters
sender_email = "your_email@gmail.com"
sender_password = "your_password"
receiver_email = "recipient@example.com"
subject = "Subject of the Email"

# Create message container
msg = MIMEMultipart()
msg['From'] = sender_email
msg['To'] = receiver_email
msg['Subject'] = subject

# Check conditions and compose email body
email_body = ""
for row in data:
    if row['Condition']:  # Add your condition here
        email_body += f"Name: {row['Name']}, Email: {row['Email']}\n"

# Add email body to the message
msg.attach(MIMEText(email_body, 'plain'))

# Send email
try:
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.starttls()
    server.login(sender_email, sender_password)
    text = msg.as_string()
    server.sendmail(sender_email, receiver_email, text)
    server.quit()
    print("Email sent successfully!")
except Exception as e:
    print(f"Error: {e}")
