import requests
from time import sleep
import threading
import socket
import uuid
import os
import ctypes
import secrets
from termcolor import colored
from colorama import init
import autopy
from discord_webhook import DiscordWebhook
from discord_webhook import DiscordEmbed
uid = uuid.uuid4()
req = requests.session()
clear = lambda: os.system('cls')
print_lock = threading.Lock()
init()
ip = socket.gethostbyname(socket.getfqdn())
ipcheck = requests.get("https://pastebin.com/raw/TsWEGY0M").text

if str(ipcheck).find(f"{ip}") != -1:
    print(f'\n[+] Welcome Sir To Senior Swap ^_^')
    sleep(0.5)

else:
    input(f"[-] Your ip Not Active > {ip}\ncontact @21181 | @s_sa")
    exit(0)
try:
    design = str(open("design.txt", "r").read())
except:
    input("[-] can't find design.txt file ")
MessageBox = design.split(':')[0]
name = design.split(':')[1]
banner = requests.get(f'http://artii.herokuapp.com/make?text={name}').text
print(colored(banner,'yellow'))
print(colored("-"*25,"green"))
print('\n')
headers = {
    'User-Agent': 'Instagram 113.0.0.39.122 Android (24/5.0; 515dpi; 1440x2416; huawei/google; Nexus 6P; angler; angler; en_US)',
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate",
    "Accept-Language": "en-US",
    "X-IG-Capabilities": "3brTvw==",
    "X-IG-Connection-Type": "WIFI",
    "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
    'Host': 'i.instagram.com',
    'Connection': 'keep-alive'
}
def discord(old,target,att):
    wesky = DiscordWebhook(
        url='https://discord.com/api/webhooks/800239035409760267/Tg3QybauV7lCUY9NuZkmsnPHQMLrxW48lE_4-oUF3rQ6WmixU5aWgdyIb_dlDHmH4TiW'
    )
    embed = DiscordEmbed(title=f'Senior Swapper\n@{target} Swapped Successfully', description='❖ Swapper by: @qy3',
                         color=9109504)
    embed.add_embed_field(name=f'❖ @{old} to @{target}', value=f'Att >> {att} ')
    embed.set_image(url='https://media.giphy.com/media/8Lc5xmvzRhlLy/giphy.gif')
    embed.set_timestamp()
    wesky.add_embed(embed)
    wesky.execute()
print('[1] Auto Swap\n[2] Normal Swap')
q = int(input('[?] Choose: '))
if q == 1:
    clear()
    print(colored(banner, 'yellow'))
    print(colored("-" * 25, "green"))
    print('\n')
    username = input('[+] Username : ')
    password = input('[+] Password : ')

    url = 'https://i.instagram.com/api/v1/accounts/login/'
    data = {
        'uuid': uid,
        'password': password,
        'username': username,
        'device_id': uid,
        'from_reg': 'false',
        '_csrftoken': 'missing',
        'login_attempt_countn': '0'
    }
    r1 = req.post(url, headers=headers, data=data)
    Cookies = r1.cookies
    loginJS = r1.json()


    def secure():
        MyPATH = loginJS['challenge']['api_path']
        url_api = 'https://i.instagram.com/api/v1' + MyPATH
        Secure = req.get(url_api, headers=headers, cookies=Cookies).json()
        mode = []
        if ('email') in Secure['step_data']:
            mode.append('[1] Email')
        elif ('phone_number') in Secure['step_data']:
            mode.append('[0] Phone')
        else:
            autopy.alert.alert('error', f"@{name}")
            exit()
        for modes in mode:
            print(modes)
        mode = input("[+] Mode : ")
        SecureData = {
            'choice': mode,
            '_uuid': uid,
            '_uid': uid,
            '_csrftoken': 'missing'
        }

        Mode = req.post(url_api, headers=headers, data=SecureData, cookies=Cookies).json()
        jax = Mode['step_data']['contact_point']
        print(f'[-] code sent to: {jax}')
        thecode = input('[+] Code: ')
        CodeData = {
            'security_code': thecode,
            '_uuid': uid,
            '_uid': uid,
            '_csrftoken': 'missing'
        }
        Send_Code = req.post(url_api, headers=headers, data=CodeData, cookies=Cookies).text
        if 'logged_in_user' in Send_Code:
            clear()
            print(colored(banner, 'yellow'))
            print(colored("-" * 25, "green"))
            print(f'[+] Login Sueccssfully > @{username}')
            pass
        else:
            autopy.alert.alert('Wrong code', f"@{name}")


    def login():
        try:
            if 'logged_in_user' in r1.text:
                clear()
                print(colored(banner, 'yellow'))
                print(colored("-" * 25, "green"))
                print(f'[+] Login Successfully > @{username}')
                pass
            elif 'challenge_required' in r1.text:
                print('[-_-] Secured')
                secure()
            elif 'two_factor_required' in r1.text:
                print('[!] Two Factor')
                exit()
            else:
                autopy.alert.alert('wrong username or password', f"@{name}")
                exit()
        except Exception:
            print('some thing wrong')
            exit()


    login()
    info_url = 'https://i.instagram.com/api/v1/accounts/current_user/?edit=true'
    get_info = req.get(info_url, headers=headers, cookies=Cookies).json()

    email3 = get_info['user']['email']
    fname = get_info['user']['full_name']
    number = get_info['user']['phone_number']

    ThreadStart = True
    url_block = 'https://i.instagram.com/api/v1/accounts/edit_profile/'
    value = {
        '_uuid': uid,
        '_uid': uid,
        'csrftoken': 'missing',
        'first_name': fname,
        'is_private': 'false',
        'phone_number': '',
        'biography': '',
        'username': f'{username}.s_sa.{secrets.token_hex(1)}',
        'gender': '',
        'email': email3,
        'external_url': ''}


    def block():
        global headers
        c = req.post(url_block, headers=headers, data=value, cookies=Cookies)
        if 'spam' in c.text:
            input('[-] Account blocked')
            exit(0)
        elif 'is_private' in c.text:
            print('[+] All good sir u can swap :)')
        elif 'need' in c.text:
            print('[+] Need Email in acc -_-')


    ask2 = input('[?] Want Check Spam ?. [y/n]: ')
    if ask2 == 'y':
        block()
    else:
        pass
    Target = input('[+] Target: ')
    Sessionid = input('[+] Sessionid Target: ')
    coooki = {'sessionid': Sessionid}
    info_url2 = 'https://i.instagram.com/api/v1/accounts/current_user/?edit=true'
    get_info2 = req.get(info_url2, headers=headers, cookies=coooki).json()

    email32 = get_info2['user']['email']
    fname2 = get_info2['user']['full_name']
    number2 = get_info2['user']['phone_number']
    url_14d2 = 'https://i.instagram.com/api/v1/accounts/edit_profile/'
    valueeee = {
        '_uuid': uid,
        '_uid': uid,
        'csrftoken': 'missing',
        'first_name': fname,
        'is_private': 'false',
        'phone_number': number,
        'biography': '',
        'username': Target,
        'gender': '',
        'email': email3,
        'external_url': ''}
    def jaxx2():
        global headers
        cx = req.post(url_14d2, headers=headers, data=valueeee, cookies=Cookies)
        if 'spam' in cx.text:
            input("[-] Account blocked i can't check !")
            exit(0)
        elif "This username isn't available. Please try another." in cx.text:
            print('[+] Not 14day ^_^')
        elif "This username isn't available." in cx.text:
            print('[-] its 14day :(')
        else:
            print('[-] unknow error')

    ask3 = input(f'[?] Want Check if @{Target} 14day ?. [y/n]: ')
    if ask3 == 'y':
        jaxx2()
    else:
        pass
    thread = int(input('[+] Threads: '))
    clear()
    print(colored(banner, 'yellow'))
    print(colored("-" * 25, "green"))
    input(f'[+] Your Target is @{Target}\n[+] Threads is [{thread}]\n[+] Press Enter For Swap')
    changeuser = f'{Target}.s_sa.{secrets.token_hex(1)}'
    value23 = {
        '_uuid': uid,
        '_uid': uid,
        'csrftoken': 'missing',
        'first_name': fname2,
        'is_private': 'false',
        'phone_number': number2,
        'biography': ' ',
        'username': changeuser,
        'gender': '',
        'email': email32,
        'external_url': ''}

    url_change = 'https://i.instagram.com/api/v1/accounts/set_username/'
    hheaders = {
        'User-Agent': 'Instagram 159.0.0.40.130 Android (25/7.1.2; 191dpi; 1024x576; google; G011A; G011A; intel; en_US; 245196047)',
        'Accept': '*/*',
    }
    def change():
            v = requests.post(url_change, headers=hheaders, data=value23, cookies=coooki)
            change = v.status_code
            if 'is_private' in v.text:
                print(colored(f"[+] Changed Username To > {changeuser}", 'green'))
                pass
            elif change == 400:
                print('[+] Changing..')
            elif 'spam' in v.text:
                print('[-] Blocked')
            else:
                input('[x] Blocked')


    url_swap = 'https://i.instagram.com/api/v1/accounts/edit_profile/'
    valueSwap = {
        '_uuid': uid,
        '_uid': uid,
        'csrftoken': 'missing',
        'first_name': fname,
        'is_private': 'false',
        'phone_number': number,
        'biography': f'- Swapped by @{name} <3 .',
        'username': Target,
        'gender': '',
        'email': email3,
        'external_url': ''}
    xr = 0


    def swap():

        global xr
        while 1:

            p = requests.post(url_swap, headers=headers, data=valueSwap, cookies=Cookies)
            go = p.status_code
            xr += 1
            if 'is_private' in p.text:
                with print_lock:
                    print(colored(f'\n[+] Swapped Successfully: @{Target}', 'yellow'))
                    if len(Target) < 5:
                        discord(username,Target,xr)
                    autopy.alert.alert(f"- {MessageBox}: @{Target} ", f"@{name}")
                    breakpoint()
                    return True
                    exit(0)

            elif go == 400:
                ctypes.windll.kernel32.SetConsoleTitleW(str(f"att= {xr}"))
            else:
                print('[x] Blocked !')
                breakpoint()
                input()
                quit()


    thredas = []
    for i in range(thread):
        x = threading.Thread(target=swap)
        o = threading.Thread(target=change)
        x.start()
        thredas.append(x)
        o.start()
        thredas.append(o)
        for i in thredas:
            i.join()
elif q == 2:
    clear()
    print(colored(banner, 'yellow'))
    print(colored("-" * 25, "green"))
    print('\n')
    username = input('[+] Username : ')
    password = input('[+] Password : ')

    url = 'https://i.instagram.com/api/v1/accounts/login/'
    data = {
        'uuid': uid,
        'password': password,
        'username': username,
        'device_id': uid,
        'from_reg': 'false',
        '_csrftoken': 'missing',
        'login_attempt_countn': '0'
    }
    r1 = req.post(url, headers=headers, data=data)
    Cookies = r1.cookies
    loginJS = r1.json()


    def secure():
        MyPATH = loginJS['challenge']['api_path']
        url_api = 'https://i.instagram.com/api/v1' + MyPATH
        Secure = req.get(url_api, headers=headers, cookies=Cookies).json()
        mode = []
        if ('email') in Secure['step_data']:
            mode.append('[1] Email')
        elif ('phone_number') in Secure['step_data']:
            mode.append('[2] Phone')
        else:
            autopy.alert.alert('error', f"@{name}")
            exit()
        for modes in mode:
            print(modes)
        mode = input("[+] Mode : ")
        SecureData = {
            'choice': mode,
            '_uuid': uid,
            '_uid': uid,
            '_csrftoken': 'missing'
        }

        Mode = req.post(url_api, headers=headers, data=SecureData, cookies=Cookies).json()
        jax = Mode['step_data']['contact_point']
        print(f'[-] code sent to: {jax}')
        thecode = input('[+] Code: ')
        CodeData = {
            'security_code': thecode,
            '_uuid': uid,
            '_uid': uid,
            '_csrftoken': 'missing'
        }
        Send_Code = req.post(url_api, headers=headers, data=CodeData, cookies=Cookies).text
        if 'logged_in_user' in Send_Code:
            clear()
            print(colored(banner, 'yellow'))
            print(colored("-" * 25, "green"))
            print(f'[+] Login Sueccssfully > @{username}')
            pass
        else:
            autopy.alert.alert('Wrong code', f"@{name}")


    def login():
        try:
            if 'logged_in_user' in r1.text:
                clear()
                print(colored(banner, 'yellow'))
                print(colored("-" * 25, "green"))
                print(f'[+] Login Successfully > @{username}')
                pass
            elif 'challenge_required' in r1.text:
                print('[-_-] Secured')
                secure()
            elif 'two_factor_required' in r1.text:
                print('[!] Two Factor')
                exit()
            else:
                autopy.alert.alert('wrong username or password', f"@{name}")
                exit()
        except Exception:
            print('some thing wrong')
            exit()


    login()
    info_url = 'https://i.instagram.com/api/v1/accounts/current_user/?edit=true'
    get_info = req.get(info_url, headers=headers, cookies=Cookies).json()

    email3 = get_info['user']['email']
    fname = get_info['user']['full_name']
    number = get_info['user']['phone_number']

    ThreadStart = True
    url_block = 'https://i.instagram.com/api/v1/accounts/edit_profile/'
    value = {
        '_uuid': uid,
        '_uid': uid,
        'csrftoken': 'missing',
        'first_name': fname,
        'is_private': 'false',
        'phone_number': '',
        'biography': '',
        'username': f'{username}.s_sa.{secrets.token_hex(1)}',
        'gender': '',
        'email': email3,
        'external_url': ''}


    def block():
        global headers
        c = req.post(url_block, headers=headers, data=value, cookies=Cookies)
        if 'spam' in c.text:
            input('[-] Account blocked')
            exit(0)
        elif 'is_private' in c.text:
            print('[+] All good sir u can swap :)')
        elif 'need' in c.text:
            print('[+] Need Email in acc -_-')


    ask2 = input('[?] Want Check Spam ?. [y/n]: ')
    if ask2 == 'y':
        block()
    else:
        pass
    Target = input('[+] Target: ')
    url_14d = 'https://i.instagram.com/api/v1/accounts/edit_profile/'
    valueeee = {
        '_uuid': uid,
        '_uid': uid,
        'csrftoken': 'missing',
        'first_name': fname,
        'is_private': 'false',
        'phone_number': number,
        'biography': '',
        'username': Target,
        'gender': '',
        'email': email3,
        'external_url': ''}


    def jaxx():

        cx = req.post(url_14d, headers=headers, data=valueeee, cookies=Cookies)
        if 'spam' in cx.text:
            input("[-] Account blocked i can't check !")
            exit(0)
        elif "This username isn't available. Please try another." in cx.text:
            print('[+] Not 14day ^_^')


    ask3 = input(f'[?] Want Check if @{Target} 14day ?. [y/n]: ')
    if ask3 == 'y':
        jaxx()
    else:
        pass
    thread2 = int(input('[+] Threads: '))
    url_swap = 'https://i.instagram.com/api/v1/accounts/edit_profile/'
    xx = 0

    valueSwap2 = {
        '_uuid': uid,
        '_uid': uid,
        'csrftoken': 'missing',
        'first_name': fname,
        'is_private': 'false',
        'phone_number': number,
        'biography': f'- Swapped by @{name} <3 .',
        'username': Target,
        'gender': '',
        'email': email3,
        'external_url': ''}

    autopy.alert.alert(f'Your Target : @{Target}\nThreads:{thread2}', f"@{name}")
    def swapp():
        global xx
        while 1:
            p = requests.post(url_swap, headers=headers, data=valueSwap2, cookies=Cookies)

            go = p.status_code
            xx += 1
            if 'is_private' in p.text:
                with print_lock:
                    clear()
                    print(colored(banner, 'yellow'))
                    print(colored("-" * 25, "green"))
                    print(colored(f'[+] Swapped Successfully: @{Target}', 'yellow'))
                    if len(Target) < 5:
                        discord(username,Target,xx)
                    autopy.alert.alert(f"- {MessageBox}: @{Target} ", f"@{name}")
                    breakpoint()
                    return True
                    exit(0)
            elif go == 400:
                ctypes.windll.kernel32.SetConsoleTitleW(str(f"att= {xx}"))
            else:
                print('[x] Blocked !')
                break


    class threads(thread2):
        thredas = []
        for i in range(thread2):
            t = threading.Thread(target=swapp)
            t.daemon = True
            t.start()
            thredas.append(t)
        for i in thredas:
            i.join()

else:
    input('[-] Wrong Number\nPress Enter To Exit.')
    exit(0)
