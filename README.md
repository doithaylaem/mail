# 1) Cài đặt  james Server  
   Mở các port IMAP 876 143 993, POP 110, SMTP  25,465, WEB ADMIN 8000 Nếu cần
![ảnh](https://github.com/user-attachments/assets/7f71d65d-e554-4a97-9ab4-5e72542b54cb)
Bản cài đặt 
https://www.mediafire.com/file/0h062mcnlnmyfan/james-server-jpa-guice.zip/file
# 2) Cấu hình domain trỏ về VPS  
![ảnh](https://github.com/user-attachments/assets/b06dc416-583e-4f71-a65f-985c66b5fd6d)

# 3) API quản trị domain, user 
```bash
    a) Domain list
curl --location 'http://123.30.48.111:8000/domains' 
    b) Add domain  
curl --location --request PUT 'http://123.30.48.111:8000/domains/gunmail.xyz' 
    c) Thêm 1 Email mới 
     curl --location --request PUT 'http://123.30.48.111:8000/users/hanv@gunmail.xyz' \
--header 'Content-Type: application/json' \
--data '{"password":"12345678"}' 
# 4) Code đọc mail 
```csharp
    using (var client = new Pop3Client())
    {
        try
        {  
            client.Connect("gunmail.xyz", 110, MailKit.Security.SecureSocketOptions.None);
            client.Authenticate("hanv@gunmail.xyz", "12345678");
            int messageCount = client.Count;
            Console.WriteLine($"Total messages: {messageCount}");
            for (int i = 0; i < messageCount; i++)
            {
                MimeMessage message = client.GetMessage(i);

                Console.WriteLine($"Subject: {message.Subject}");
                Console.WriteLine($"From: {message.From}");
                Console.WriteLine($"Date: {message.Date}");
                Console.WriteLine("--------------------------------------------");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"An error occurred: {ex.Message}");
        }
        
        finally
        {
            // Disconnect from the server
            client.Disconnect(true);
        }
    }
}

Subject: hanv@gunmail.xyz
From: "Van Ng" <nvhabk@gmail.com>
Date: 15/10/2024 8:39:19 AM +07:00
--------------------------------------------
Subject: [Bizfly Cloud] Xác th?c email.
From: "Bizfly Cloud Customer Service" <no-reply@bizflycloud.vn>
Date: 15/10/2024 8:41:26 AM +07:00
--------------------------------------------
Subject: Xin chào m?ng anh/ch? hanv@gunmail.xyz dã d?n v?i Bizfly Cloud
From: "Bizfly Cloud" <newsletter@bizflycloud.vn>
Date: 15/10/2024 1:43:01 AM +00:00
--------------------------------------------

