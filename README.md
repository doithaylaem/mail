1) Cài đặt  james Server
2) **Cấu hình domain trỏ về VPS**
3) API quản trị domain, user  
    <br> a) Domain list curl --location 'http://123.30.48.111:8000/domains' <br>
    b) Add domain  curl --location --request PUT 'http://123.30.48.111:8000/domains/**gunmail.xyz**' <br>
    c) Thêm 1 Email mới  <br>
     curl --location --request PUT 'http://123.30.48.111:8000/users/**hanv@gunmail.xyz**' \ <br>
    --header 'Content-Type: application/json' \ <br>
    --data '{"password":"12345678"}' <br>
    

4) **Code đọc mail **
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
