# Knowledgebase
I built a knowledge base from scratch

<img width="802" height="464" alt="Screenshot 2026-08-21 144041" src="https://github.com/user-attachments/assets/94aa9bf4-2811-40bb-84e9-71943ad10758" />


Building a Knowledgebase

Building an IT Knowledge Base with Next.js, FastAPI, and PostgreSQL I recently started building an IT troubleshooting knowledge base from scratch. My goal was to create a system where I could organize IT support articles into categories such as Hardware, Software, and Process, while eventually allowing users to search, read, and manage troubleshooting information through a web interface. This article documents what I built so far and the steps I took to get the first working version running on Windows 11.

What I Wanted to Build
I wanted an IT knowledge base that could eventually contain articles such as:

<img width="157" height="71" alt="1" src="https://github.com/user-attachments/assets/077a5292-2b18-48f9-bcf6-3ab2e75f42a9" />



I wanted the system to be more than a collection of documents. I wanted a proper application with a database, **API**, and web interface.
The basic architecture I chose was:

<img width="101" height="133" alt="2" src="https://github.com/user-attachments/assets/bef88db7-eb32-4d93-92af-847fe8b64e6f" />

    
I built each layer separately and then connected them together.

## Setting Up the Backend

I started on a Windows 11 laptop and created a project directory: it-knowledge-base └── backend Inside the backend directory, I created a Python virtual environment: backend └── .venv The virtual environment gives the project its own Python packages without interfering with other Python projects on my computer. I then created: backend └── main.py I used FastAPI for the backend because I wanted a lightweight Python **API** that would be easy to develop and test. I am also still a beginner. I ran the application with: python -m uvicorn main:app --reload This started the **API** at: [http://**127**.0.0.1:**8000**](http://**127**.0.0.1:**8000**) FastAPI also automatically provided an interactive **API** documentation page at: [http://**127**.0.0.1:**8000**/docs](http://**127**.0.0.1:**8000**/docs) This became extremely useful for testing the **API** while I was developing it.

## Installing PostgreSQL

Next, I installed PostgreSQL 18. Initially, I couldn't run: psql --version because Windows reported that psql was not recognized. I discovered that PostgreSQL was installed under: C:\Program Files\PostgreSQL\18\bin I added that directory to my Windows **PATH**. Even though I had some issues getting the **PATH** recognized immediately, PostgreSQL itself was installed correctly, so I continued using pgAdmin 4 to manage the database.

<img width="750" height="501" alt="Screenshot 2026-08-21 143953" src="https://github.com/user-attachments/assets/fe0b6ac0-d8a5-4521-bf81-c7940fbc0d26" />



## Creating the Database

Using pgAdmin, I created a PostgreSQL database called: it_knowledge My initial database structure was designed around categories, articles, and tags. The tables were: it_knowledge │ ├── categories ├── articles ├── tags └── article_tags The categories table contains the main knowledge-base categories. I created: Hardware Software Process I then verified them with: **SELECT** * **FROM** categories; and confirmed that all three categories existed.

## Creating the Articles Table

The articles table stores the actual knowledge-base content.
My initial structure included:
<img width="311" height="218" alt="3" src="https://github.com/user-attachments/assets/154cc3e0-9719-41ed-9247-5edba8d9272c" />



    <img width="682" height="420" alt="Screenshot 2026-08-21 091243" src="https://github.com/user-attachments/assets/6d153878-b08c-4dcd-97cf-a43478ff1574" />


## Connecting Python to PostgreSQL

I then connected my FastAPI application to PostgreSQL. I installed: pip install psycopg2-binary python-dotenv I used psycopg2 to communicate with PostgreSQL and python-dotenv so that I could keep my database connection information out of my Python source code. I created a .env file inside the backend directory to store settings, sensitive information and other environmental varibles: backend ├── .env ├── .venv └── main.py The .env file contains my database connection string: DATABASE_URL=postgresql://postgres:YOUR_PASSWORD@localhost:**5432**/it_knowledge I replaced YOUR_PASSWORD with the password I created when I installed PostgreSQL. I also made sure not to publish my actual database password.

<img width="653" height="320" alt="Screenshot 2026-08-21 094620" src="https://github.com/user-attachments/assets/c1a4fa95-b322-4aba-b7c7-1d7116947780" />



## Building the FastAPI API

I created several **API** endpoints.
The first was a basic health check:

<img width="281" height="239" alt="4" src="https://github.com/user-attachments/assets/394f6505-a393-4a28-b5e7-c04f7bd4613b" />


<img width="943" height="470" alt="Screenshot 2026-08-21 144005" src="https://github.com/user-attachments/assets/11895674-b8c3-4a0b-9cf3-1b8f8b2ba2ad" />



## Creating Articles Through the API

I then created a **POST** /articles endpoint.
This allows me to create an article through the **API**.
For example:
<img width="253" height="121" alt="5" src="https://github.com/user-attachments/assets/9559d876-6203-4ed6-817b-1de24069d685" />



That was my first real knowledge-base article.

## Retrieving Articles

Next, I created:
**GET** /articles
This endpoint queries PostgreSQL and joins the articles with their categories.
The **API** can now return information such as:

<img width="157" height="186" alt="6" src="https://github.com/user-attachments/assets/d220b9d5-93b3-4d67-8e89-441325ddc766" />



<img width="453" height="401" alt="Screenshot 2026-08-21 143932" src="https://github.com/user-attachments/assets/53a6bf77-2004-4d8d-8fea-038327efc55f" />


## Building the Frontend

Once the backend was working, I created a frontend using Next.js. From my project directory, I ran: npx create-next-app@latest frontend This gave me: it-knowledge-base │ ├── backend │ └── frontend I started the frontend with: npm run dev The Next.js application became available at: [http://localhost:**3000**](http://localhost:**3000**) Initially, I saw the standard Next.js starter page. I replaced that page with my own IT Knowledge Base interface.

## Designing the Knowledge Base Homepage

I created a homepage containing:
   
    <img width="202" height="255" alt="7" src="https://github.com/user-attachments/assets/5574648d-7cec-4239-8061-6f061991b0bd" />


<img width="431" height="257" alt="Screenshot 2026-08-21 144054" src="https://github.com/user-attachments/assets/5fd0c9cb-cd56-4880-96d9-7ac98e475052" />



## Connecting Next.js to FastAPI

Initially, the article on the homepage was hard-coded.
That wasn't what I wanted.
I wanted the website to retrieve its information from the database.
I therefore added code to the Next.js application that calls:
[http://**127**.0.0.1:**8000**/articles](http://**127**.0.0.1:**8000**/articles)
The flow became:

<img width="104" height="191" alt="8" src="https://github.com/user-attachments/assets/45d52020-4825-4a59-b023-6c7f651c235a" />


I also added **CORS** configuration to FastAPI so that the Next.js application running on port **3000** could communicate with the FastAPI application running on port **8000**.

## The Result

At this point, I had successfully connected all three major components.
   <img width="109" height="259" alt="9" src="https://github.com/user-attachments/assets/059218a9-476b-45e6-9aef-091b53e59ff2" />


    
Most importantly, when I created the Laptop Will Not Power On article in PostgreSQL, the article appeared automatically on my Next.js website.
That proved that the entire data pipeline was working.

<img width="521" height="472" alt="Screenshot 2026-08-21 144027" src="https://github.com/user-attachments/assets/eeb7cb83-49cb-40f0-a66b-7f6758e110a2" />


What I Have Built So Far
My project currently looks like:
it-knowledge-base

<img width="104" height="152" alt="10" src="https://github.com/user-attachments/assets/05bd3b66-b023-4560-a237-06daaf1f0176" />


The backend provides:

<img width="183" height="117" alt="11" src="https://github.com/user-attachments/assets/83c3206b-bcac-4c75-bd31-5fc9efb7b252" />


What I Plan to Build Next This is only the foundation. My next goal is to turn this into a full IT support knowledge platform. Some of the features I plan to add include: Better article structure Instead of storing everything in one content field, I want articles to have structured sections: Article ├── Title ├── Category ├── Subcategory ├── Symptoms ├── Environment ├── Problem ├── Prerequisites ├── Troubleshooting Steps ├── Resolution ├── Verification ├── Escalation Criteria ├── Tags ├── Author └── Last Reviewed Search I want users to be able to search for things like: **VPN** won't connect and find the appropriate troubleshooting articles. Category navigation Users should be able to browse: Hardware ├── Laptops ├── Desktops ├── Printers └── Networking

Software ├── Windows ├── Microsoft **365** ├── **VPN** └── Applications

<img width="304" height="177" alt="12" src="https://github.com/user-attachments/assets/1e99bdc0-0100-47d3-ace7-85fbc217df26" />


I also plan to add authentication so that regular users can read articles while IT staff can manage the knowledge base.

<img width="661" height="230" alt="Screenshot 2026-08-21 144116" src="https://github.com/user-attachments/assets/bf5df129-9423-4226-a619-54d600da4bd2" />


### Lessons Learned

One of the biggest lessons from building this was that I didn't need to build the entire application at once.
I broke the project into layers:
## Python environment
       ↓
## FastAPI
       ↓
## PostgreSQL
       ↓
## Database tables
       ↓
## API endpoints
       ↓
## Next.js frontend
       ↓
## Connect frontend to API
That made it much easier to troubleshoot when something went wrong.
I also learned how important indentation is when working with Python. A single indentation error can prevent the entire FastAPI application from starting. I learned this by asking AI, because I just got stumped and there is no one here I can ask. 
Another important lesson was to keep configuration information, such as database passwords, out of the source code by using a .env file. I learned how environmental variables work and how to use them.

### Final Result

I started with an idea for an IT troubleshooting knowledge base. I now have the foundation of a real application: Next.js provides the user interface. FastAPI provides the backend **API**. PostgreSQL stores the knowledge-base data. The three components are communicating successfully, and my first real troubleshooting article is being retrieved from the database and displayed on the website. This gives me a solid foundation to continue building a more complete IT knowledge management platform. The next major step is turning the single article preview into a full article system with structured troubleshooting content, individual article pages, search, and category navigation.
