---
title: "Creating Sub-domain in Google domains"
datePublished: Wed Aug 16 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clmfx15nu000s08jz6u1wbh5k
slug: creating-sub-domain-in-google-domains
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/zhZydTyNMPg/upload/2de7f6fb08454629fb096f10da7ebd50.jpeg
tags: dns, google, devops, map-subdomain

---

To create a subdomain in Google Domains, you'll need to add DNS records for the subdomain in your domain's DNS settings. Here's a step-by-step guide on how to do this:

1. **Log in to Google Domains:** Go to the Google Domains website ([https://domains.google.com/](https://domains.google.com/)) and log in to your Google account if you haven't already.
    
2. **Select Your Domain:** Click on the domain name for which you want to create a subdomain. This will take you to the domain management dashboard.
    
3. **Access DNS Settings:** In the left sidebar, click on "DNS." This is where you'll manage your domain's DNS records.
    
4. **Add a DNS Record for the Subdomain:** To create a subdomain, you typically add either an `A` or `CNAME` record, depending on your specific needs.
    
    * **Creating a CNAME Subdomain:**
        
        * If you want to create a subdomain that points to another domain or hostname, such as "[subdomain.example.com](http://subdomain.example.com)" pointing to "[www.example.com](http://www.example.com)," you can add a `CNAME` record.
            
        * Click the "+ Add" button next to "Custom resource records."
            
        * In the "Name" field, enter the subdomain you want to create (e.g., "subdomain").
            
        * In the "Type" field, select "CNAME."
            
        * In the "Data" field, enter the domain or hostname to which the subdomain should point (e.g., "[www.example.com](http://www.example.com)").
            
        * Click the "Add" button to save the `CNAME` record.
            
    * **Creating an A Subdomain:**
        
        * If you want to create a subdomain that has its own IP address, you can add an `A` record.
            
        * Click the "+ Add" button next to "Custom resource records."
            
        * In the "Name" field, enter the subdomain you want to create (e.g., "subdomain").
            
        * In the "Type" field, select "A."
            
        * In the "Data" field, enter the IP address to which the subdomain should point.
            
        * Click the "Add" button to save the `A` record.
            
5. **Save Your Changes:** After adding the DNS record for your subdomain, make sure to click the "Save" or "Save Changes" button in the DNS settings. Your subdomain is now created and should be functional once DNS propagation is complete.
    

Please note that DNS changes, including the creation of subdomains, can take some time to propagate across the internet. It may take several hours or up to 48 hours for the subdomain to start working correctly. You can use online DNS lookup tools to check the DNS records for your subdomain and verify that it's resolving correctly once propagation is complete.