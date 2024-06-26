import tkinter as tk
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

def send_email(receiver_email, subject, body):
    sender_email = "your_email@example.com"  # อีเมล์ผู้ส่ง
    password = "your_email_password"  # รหัสผ่านอีเมล์ผู้ส่ง

    message = MIMEMultipart()
    message["From"] = sender_email
    message["To"] = receiver_email
    message["Subject"] = subject

    message.attach(MIMEText(body, "plain"))

    with smtplib.SMTP_SSL("smtp.example.com", 465) as server:
        server.login(sender_email, password)
        server.sendmail(sender_email, receiver_email, message.as_string())

def send_leave_request():
    employee_name = name_entry.get()
    start_date = start_date_entry.get()
    end_date = end_date_entry.get()
    hours = hours_entry.get()
    day_off = day_off_entry.get()

    department_head_email = "department_head@example.com"
    division_head_email = "division_head@example.com"
    leave_details = f"Employee: {employee_name}\nStart Date: {start_date}\nEnd Date: {end_date}\nHours: {hours}\nDay Off: {day_off}"
    send_email(department_head_email, "Leave Request", leave_details)
    send_email(division_head_email, "Leave Request", leave_details)

    hr_email = "hr@example.com"
    hr_message = f"Leave request for employee {employee_name} has been sent.\nDetails:\n{leave_details}"
    send_email(hr_email, "Leave Request", hr_message)
    status_label.config(text="Leave request sent successfully!")

root = tk.Tk()
root.title("Leave Request System")

name_label = tk.Label(root, text="Employee Name:")
name_label.grid(row=0, column=0, padx=5, pady=5)
name_entry = tk.Entry(root)
name_entry.grid(row=0, column=1, padx=5, pady=5)

start_date_label = tk.Label(root, text="Start Date (YYYY-MM-DD):")
start_date_label.grid(row=1, column=0, padx=5, pady=5)
start_date_entry = tk.Entry(root)
start_date_entry.grid(row=1, column=1, padx=5, pady=5)

end_date_label = tk.Label(root, text="End Date (YYYY-MM-DD):")
end_date_label.grid(row=2, column=0, padx=5, pady=5)
end_date_entry = tk.Entry(root)
end_date_entry.grid(row=2, column=1, padx=5, pady=5)

hours_label = tk.Label(root, text="Number of Hours:")
hours_label.grid(row=3, column=0, padx=5, pady=5)
hours_entry = tk.Entry(root)
hours_entry.grid(row=3, column=1, padx=5, pady=5)

day_off_label = tk.Label(root, text="Day Off:")
day_off_label.grid(row=4, column=0, padx=5, pady=5)
day_off_entry = tk.Entry(root)
day_off_entry.grid(row=4, column=1, padx=5, pady=5)

send_button = tk.Button(root, text="Send Leave Request", command=send_leave_request)
send_button.grid(row=5, column=0, columnspan=2, padx=5, pady=5)

status_label = tk.Label(root, text="")
status_label.grid(row=6, column=0, columnspan=2, padx=5, pady=5)

root.mainloop()
