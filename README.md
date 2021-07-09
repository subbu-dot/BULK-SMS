# BULK-SMS

package com.roytuts.java.bulk.email.send;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.URL;
import java.net.URLConnection;
import java.net.URLEncoder;

public class SMSSender {

  /**
   * Send a 1-way SMS from "XYZCorp"
   *
   * @param String[] Command line arguments
   */
  public static void main(String[] args) {
    
    // Declare the security credentials to use
    String username = "Suhaib";
    String password = "7904050818";

    // Set the attributes of the message to send
    String message  = "Hello World";
    String type     = "1-way";
    String senderid = "Subbu";
    String to       = "7904050818";

    try {

      // Build URL encoded query string
      String encoding = "UTF-8";
      String queryString = "username=" + URLEncoder.encode(username, encoding)
        + "&password=" + URLEncoder.encode(password, encoding)
        + "&message=" + URLEncoder.encode(message, encoding)
        + "&senderid=" + URLEncoder.encode(senderid, encoding)
        + "&to=" + URLEncoder.encode(to, encoding)
        + "&type=" + URLEncoder.encode(type, encoding);

      // Send request to the API servers over HTTPS
      URL url = new URL("https://api.directsms.com.au/s3/http/send_message?");
      URLConnection conn = url.openConnection();
      conn.setDoOutput(true);
      OutputStreamWriter wr = 
        new OutputStreamWriter(conn.getOutputStream());
      wr.write(queryString);
      wr.flush();

      // The response from the gateway is going to look like this:
      // id: a4c5ad77ad6faf5aa55f66a
      // 
      // In the event of an error, it will look like this:
      // err: invalid login credentials
      BufferedReader rd = new BufferedReader(
        new InputStreamReader(conn.getInputStream()));
      String result = rd.readLine();
      wr.close();
      rd.close();

      if(result == null) {
        System.out.println("No response received");
      }
      else if(result.startsWith("id:")) {
        System.out.println("Message sent successfully");
      } 
      else {
        System.out.println("Error - " + result);
      }
    } 
    catch (Exception e) {
      System.out.println("Error - " + e);
    }
  }
}
