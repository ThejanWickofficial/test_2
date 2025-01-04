#Thejan WickramaArachchi
#2024/DEC/31
#Trade report maker
import re
from datetime import datetime

# Format numbers with spaces between thousands, millions, etc.
def format_number(num):
    return f"{num:,.2f}".replace(",", " ")

# Inputs
name = input("Name of the company: ")
name_sanitized = re.sub(r'[<>:\"/\\|?*]', '_', name)  # Sanitize file name
share = int(input("Number of shares you brought: "))
date1_str = input("\nEnter the brought date (YYYY-MM-DD): ")
b_price = float(input("Brought price of one share: "))
date2_str = input("\nEnter the sold date (YYYY-MM-DD): ")
s_price = float(input("Sold price of one share: "))

# Process
b_g_amount = round((share * b_price), 2)
b_brok = round(((b_g_amount / 100) * 1.12), 2)
b_net_amount = round((b_g_amount + b_brok), 2)

s_g_amount = round((share * s_price), 2)
s_brok = round(((s_g_amount / 100) * 1.12), 2)
s_net_amount = round((s_g_amount - s_brok), 2)

prof = round((s_net_amount - b_net_amount), 2)

# Convert the input strings to datetime objects
date1 = datetime.strptime(date1_str, "%Y-%m-%d")
date2 = datetime.strptime(date2_str, "%Y-%m-%d")
# Calculate the difference between the two dates
diff = abs((date2 - date1).days)

pdp = round((prof / diff), 2)
app = round(((pdp * 100 * 365) / b_net_amount), 2)

# Prepare the display content
output = (
    f"\n"
    f"\n{name_sanitized}"
    f"\n---Date---|---Type---|---Price---|---Gross Amount---|---Brokerage---|---Net Amount---|---Profit---|---PDP---|---APP---|"
    f"\n{date1_str} |____B____|      {format_number(b_price)} |        {format_number(b_g_amount)} |        {format_number(b_brok)} |      {format_number(b_net_amount)} |"
    f"\n{date2_str} |____S____|      {format_number(s_price)} |        {format_number(s_g_amount)} |        {format_number(s_brok)} |      {format_number(s_net_amount)} |   {format_number(prof)} |    {format_number(pdp)}|  {format_number(app)}% |"
)

# Print the output to console
print(output)

# Save the output to a text file named as the sanitized company name
file_name = f"{name_sanitized}.txt"
with open(file_name, "w") as file:
    file.write(output)

print(f"\nThe output has been saved to {file_name}")
