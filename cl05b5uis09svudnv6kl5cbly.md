## How to point your sub-domains to multiple projects or websites hosted on multiple platforms

So, I had a problem recently.

Before that, here is some context for beginners.

To build your own website and make it accessible on the internet, you need a few things:
- A code repository of your website, generally a private git repo. This can be outsourced if you are using some platform like WordPress, Hashnode, or any other blog/website builder.
- A hosting platform that builds and deploys your site on their servers. These include platforms like Heroku, Netlify, Vercel, etc. Custom website builders also provide automatic hosting services.
- You need to connect your code repository/website to these hosting platforms and deployment is generally automatic. The websites are hosted on the platform subdomains.
- If you want to point them to your own custom domain, you have to configure the DNS records accordingly. You can purchase a custom domain via various vendors like Namecheap, GoDaddy, Google Domains, etc.

---

In this post, I have explored some insights on configuring these custom domain records to point at our web deployments involving multiple platforms and subdomains.

I set up a new blog on Hashnode and wanted to host it via my custom subdomain. My domain provider was Namecheap. The process was straightforward.
- In your blog dashboard you have a domain option where you type the domain/subdomain you own.
- I entered `blog.xqbuilds.com` as that's where I want the blog pointed at.
- It then gives us a **CNAME record** we should add in your host records in Namecheap's advanced DNS settings.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1645959856536/FoGbhPrd9.png)

- You will see an "add record" option. You can select CNAME and set host to `blog` and value to `hashnode.network`.
- I saved the settings and within a couple of minutes, the blog went live @ blog.xqbuilds.com.

So, all good?

Well, well, well. A problem arrived soon.

<img src="https://c.tenor.com/9z8aTaVmPfwAAAAi/cats-sad.gif" alt="Sad Kitty"/>

I wanted to host my main website with Vercel at `xqbuilds.com`. Hashnode uses Vercel too and we cannot use the same domain after it is used with Hashnode. There is an easy bypass for this as detailed in an article.

%[https://support.hashnode.com/docs/mapping-domain-on-both-hashnode-and-vercel]

I did the same. Deleted the custom subdomain entry in Hashnode and added the main domain to a test app in Vercel. Below is the image of Vercel's project settings page where you can add custom domains.
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1645961602843/HSBAVqVCs.png)

There were a few inconsistencies here w.r.t the documentation and blogs I referred to. So, I thought to write about what worked for me.

- We have to enter the full domain name with `www`. So, in my case, I would enter `www.xqbuilds.com`.
- Vercel will automatically add `xqbuilds.com` and try to redirect it to the main domain.
- This causes a bunch of issues. It will also cause an issue if you just directly add it without `www`.
- Vercel will mostly start showing errors saying some A record was not configured properly. You can actually delete the entry without `www`.
- Go back to Namecheap's advanced DNS settings and add a new CNAME host record with host set to `www` and value set to `cname.vercel-dns.com`.
- This should work after a couple of minutes of waiting. Certificates are automatically generated and set for you.
- I would recommend one to always use CNAME records to point the domains and subdomains to the required routes.
- I did tamper with the A-records and named servers options which actually inspired me to write this blog. Do not do that. Seriously, **DONT**. In my case, everything broke down.
- While accessing the domain, if you get some no domain found error or the page fails to load, you can try accessing it via your smartphone. If it works there, then it's a client-side DNS error. For your computer, you may have to reset the DNS cache or change some configs to use a public DNS resolver like Google's. There are good resources online for this.
- Now, you can go back to Hashnode and add your subdomain to point to your blog. You will get an email to let Vercel give control to Hashnode.

We have just set up `www.xqbuilds.com` to host a separate Vercel project while `blog.xqbuilds.com` hosts the Hashnode blog.

## What if you want to add more subdomains to point to your webpages hosted elsewhere?

To make the best use of free tiers in hosting platforms, I wanted to see if I can point other subdomains to projects hosted elsewhere, say in Netlify.

CNAME host records to the rescue!

Turns out that you can just add a new CNAME record with host set to the subdomain and value to the URL link to the Netlify app. I used a past project I deployed via Netlify to test. Here are some of my CNAME records.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1645964579338/MQw1EcPdk.png)

Note that you have to add this subdomain on the Netlify dashboard for your project too. The Netlify record seamlessly helped me point `app.xqbuilds.com` to the click-to-tweet web app deployment. This way, you can easily use multiple different tech stacks for your subdomains and for different utilities.

Note that you can add more subdomains under Vercel too.

What errors or roadblocks did you face while setting up your website? Let me know in the comments and I will update the solutions in the blog post to keep it fresh and working.

<img src="https://c.tenor.com/UF9ihhR8aRoAAAAi/fox-thanky.gif" alt="Thank you!"/>
