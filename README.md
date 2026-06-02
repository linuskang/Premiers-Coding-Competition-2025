> [!NOTE]
> # ATTENTION!
> Bubbly, an app I designed for people to find their nearest water fountain anywhere in Australia, was my award winning submission for the 2025 Premier Coding Competition QLD. This repository has since been archived however I've decided to continue maintaining the project. You can view the new repository at [bubblymaps/bubblymaps](https://github.com/bubblymaps/bubblymaps).
> 
> The updated site url is at https://bubblymaps.org
> 
> Below is the original README

## Bubbly: Mapping out Australia's water fountains

<img width="2556" height="1274" alt="image" src="https://github.com/user-attachments/assets/5f732818-b3c7-4ec8-aa6b-c344a211f938" />

**Bubbly** is an all-in-one app designed to help you locate your nearest water fountain, with our code open sourced. This was my submission for the *2025 Premier Coding Comp* in Australia.

You can access **Bubbly** at https://bubbly.linuskang.au.

## About the platform

I first realised the challenge of accessing clean water at this year’s Ekka, when I struggled to find a water bubbler to refill my bottle. This experience made me wonder why there wasn’t a simple, reliable way to locate water fountains in public spaces.

After researching, I discovered two major problems with existing solutions:
- Most platforms rely on static or community-generated databases that are rarely maintained, leading to inaccurate locations.
- Many existing tools have clunky interfaces or lack mobile optimisation, making them difficult to use in the moment.
- While there may be site maps of fountains for locations like malls, they are very limited in range, normally just detailing a specific area.

These issues make it unnecessarily hard for everyday people to find nearby water bubblers quickly and confidently.

## My solution

I developed a platform called Bubbly, which aims to centralise and simplify access to clean drinking water across Australia. Through my app, users can:
- Search for nearby bubblers using an intuitive, map-based interface.
- Check reviews and condition reports of existing bubblers to know if they are functional and clean.
- Contribute by adding new bubblers or updating the status of existing ones, ensuring the database stays accurate and up to date.

These are also the other features:
- Bookmarking fountians.
- AI Chatbot for questions.
- Viewing (guest) and posting reviews.
- Viewing contribution logs.
- Amenities viewer.
- Logging using Discord and Seq.
- User profile systems.
- XP tiering systems for accounts
- Report abuse system for reviews, waypoints, and users.

Below are some videos of my app features:

The overview of each water fountain. Shows essential information.

<img width="2000" height="1500" alt="image" src="https://github.com/user-attachments/assets/839c6cac-2a0c-4aee-a7ef-c0957fd1a107" />

<img width="399" height="754" alt="image" src="https://github.com/user-attachments/assets/2b6cb4a3-f880-435b-85bc-9e761d119e5b" />

This tab shows user reviews.

<img width="412" height="746" alt="image" src="https://github.com/user-attachments/assets/f167da5b-4534-46ed-b19a-d7c7c9297cf9" />

This tab lists all the changes that users have made/contributed to the database. Basically a commits log.

<img width="410" height="946" alt="image" src="https://github.com/user-attachments/assets/13c1adac-9cda-4beb-9c24-6f2fcc645df1" />

This is the star of all of Bubbly's features: an AI assistant. You can ask it for details about the bubbler, summarise user reviews, and ask it questions like "Can my dog drink water here, is it dog friendly". We use Ollama + our own custom agent functions to call the Bubbly API for data.

<img width="405" height="742" alt="image" src="https://github.com/user-attachments/assets/63fffd7f-5601-4496-80be-9660c35c5951" />

## Self-hosting Bubbly

You can easily get started hosting the production ready version of Bubbly by hosting the app via. docker. Follow the below steps to get started:

```bash
git clone https://github.com/linuskang/bubbly
cd bubbly/App
nano docker-compose.yml
```

Edit the ``docker-compose.yml`` file @ ``bubbly/App/docker-compose.yml`` to your own values.

```yaml
version: "3.9"

services:
  bubbly:
    image: ghcr.io/linuskang/bubbly/bubbly:latest
    container_name: bubbly
    ports:
      - "3400:3400"
    environment:
      DATABASE_URL: "mysql://user:password@localhost:3306/bubbly" # change to your database creds
      NEXTAUTH_SECRET: "0KprtXWjpbLzXAnNR+BUREhglOjCUvnjIMki5Uq+JAs=" # change to whatever you want.
      NEXTAUTH_URL: "https://your_app_url_to_access_bubbly"
      RESEND_API_KEY: "your_resend_api_key" # https://resend.com
      API_KEY: "change_this_to_whatever_you_want"
      OLLAMA_API_URL: "https://agent.linus.id.au" # use my prebuilt agent ai server
      DISCORD_WEBHOOK_URL: "https://your_discord_webhook" # https://discord.com/developers
      SEQ_URL: "https://your_seq_logging_server" # https://datalust.co
    restart: unless-stopped
```

Pull the docker image and start it via:

```bash
sudo docker-compose up -d
```

Access Bubbly at http://your_ip:3400

## Docs for developers

API documentation is available at https://bubbly.linuskang.au/docs or your own self hosted instance /docs.

## Tech stack

- Next.js - App framework
- Auth.js - Authentication
- Resend - Emails
- MySQL Server w/ Prisma
- MapLibre GL - Map library
- Shadcn/ui - UI framework
- TileServer GL - Map tile server

Below are detailed diagrams of how Bubbly is set up and maintained, including our software and hardware layouts.

<img width="714" height="647" alt="image" src="https://github.com/user-attachments/assets/dcc43d4c-3c6b-45ef-9a0f-ca255781069c" />
<img width="717" height="661" alt="image" src="https://github.com/user-attachments/assets/c159365b-c243-4ff3-b304-4f8e88c25ddb" />
<img width="992" height="663" alt="image" src="https://github.com/user-attachments/assets/e90b6e29-1c9d-474c-bcb6-008d7d558495" />

## What's different about my platform from others?

Bubbly is different from other water fountain locator tools because it is a user-driven platform where people contribute and edit the database, with a trust system that ensures only reliable users can make changes, keeping the information accurate and up to date. This means that Bubbly doesn't rely on a single endpoint for recieving waypoint data. It uses multiple verified sources mixed together to create our huge dataset. We've changed up the Bubbler Map space by allowing verified users with enough XP from reviews to start contributing to the platform, allowing them to edit and add water bubblers. This helps us keep our information heavily updated so we can get reliable data. I personally source and import accurate datasets from across the internet, then manually verify their location and details. Verified entries are marked as admin verified, giving users confidence that they can rely on the data.

But accuracy doesn’t stop there. Because things are constantly changing, I cant update everything quickly as it happens, so Bubbly is designed to be community-driven:

- Any user can add new bubblers, edit incorrect details, or leave reviews about water quality, accessibility, and condition.

- Contributions earn XP. As users gain XP, their edits are trusted more and require less review, gradually unlocking new editing privileges.

- All contributions are scanned for spam, abuse, or inaccurate info before they go live. Suspicious submissions are flagged for manual review.

- Each contribution is linked to the user who made it, building a public contribution history to encourage responsibility and transparency.

- Edits are versioned so incorrect data can be rolled back if necessary, ensuring long-term accuracy.

In the long term, I'm heavily aiming to become the world’s most detailed, updated, community driven repository of drinking water locations. Beyond simply mapping bubblers, the platform will provide open data access for governments, event organizers, and developers to integrate into their services, helping cities plan maintenance and improve accessibility. My included gamification elements like badges, leaderboards, and community ranks will encourage sustained contributions, while analytics dashboards will give insights into water quality, usage trends, and infrastructure gaps. Ultimately, Bubbly envisions a future where everyone has easy access to safe, reliable drinking water while reducing single-use plastic consumption and supporting sustainable communities globally.

### Early beta user feedback

I invited 50 randomly selected students from Calamvale Community College to test the platform. The response was overwhelmingly positive with 90% of users said the platform was a smart idea. Some participants raised concerns that a user-contribution system could be abused by bad actors. To address this, I implemented:

- An XP tiering system that gradually unlocks editing permissions as users contribute accurate information over time.

- AI-powered moderation to detect and filter out inappropriate or misleading reviews before they are published.

## Sustainability towards the future

Looking forward, Bubbly has the potential to scale into an enterprise platform by partnering with:

- Local councils and governments

- Schools and universities

- Event organisers

- Public venue and transport management groups

These partnerships could ensure an accurate, nationwide database of water bubblers is maintained. Furthermore, the platform is designed to be extensible: future updates are planned to include locating public toilets, as well as other essential amenities.

## License

**Bubbly** is under **CC BY-NC 4.0**. See [LICENSE](LICENSE) for more details.

## Credit

**Bubbly** is a project by **Linus Kang**. For any enquiries, please reach out at **email@linus.id.au**


























