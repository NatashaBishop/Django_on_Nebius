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
   
3. The Database Structure (models.py)  
I need to link the user's "Internal Token" balance to their AI Trader's "Search" instructions.
```
from django.db import models
from django.contrib.auth.models import User

class Profile(models.Model):
    user = models.ForeignKey(User, on_extended_class=True)
    token_balance = models.DecimalField(max_digits=10, decimal_places=2, default=0.00)

class MarketplaceOffer(models.Model):
    seller = models.ForeignKey(User, on_delete=models.CASCADE)
    item_name = models.CharField(max_length=200)
    asking_price = models.DecimalField(max_digits=10, decimal_places=2)
    description = models.TextField()

class TraderTask(models.Model):
    owner = models.ForeignKey(User, on_delete=models.CASCADE)
    target_item = models.CharField(max_length=200)
    max_price = models.DecimalField(max_digits=10, decimal_places=2)
    status = models.CharField(max_length=20, default='searching') # searching, negotiating, completed
```
