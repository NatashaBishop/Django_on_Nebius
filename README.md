 a sample Django + Celery setup showing how an AI trader "wakes up" to look for a deal:  
 To keep costs down and ensure my web app is fast, I am using Django for the marketplace logic and  
 Celery to handle the AI traders in the background.  
 This prevents the website from freezing while a "trader" is thinking or negotiating.  
 
 ## The simplified architecture and code structure.
1. The Stack

    Django: Manages users, tokens, and the marketplace database.  
    Celery: A task queue that runs the "AI Trader" logic in the background.  
    Redis: The "postman" that delivers tasks from Django to Celery.  
    Nebius AI Studio: The brain (Llama 3.1 8B) accessed via API.  
