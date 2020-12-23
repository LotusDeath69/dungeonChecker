import requests
import string
import nbt
import io
import base64

api = ''
username = input('username?: ')
mojangapi = f'https://api.mojang.com/users/profiles/minecraft/{username}?'
response = requests.get(mojangapi)
uuid = response.json()['id']

profile_number = requests.get(f'https://api.hypixel.net/skyblock/profiles?key={api}&uuid={uuid}').json()
profiles = []
for i in profile_number['profiles']:
    profiles.append(i['profile_id'])

profile_name = requests.get(f'https://api.hypixel.net/skyblock/profiles?key={api}&uuid={uuid}').json()
cute_name = []
for f in profile_name['profiles']:
    cute_name.append(f['cute_name'])


def loreprocessor(str):
    characters = ['0','1','2','3','4','5','6','7','8','9','a','b','c','d','e','f','g','k','l','m','n','o']
    x = 0
    while x <= len(characters)-1:
        str = str.replace('§'+characters[x],'')
        str = str.replace('RIGHT CLICK','')
        str = str.replace('This item can be reforged!','')
        x = x+1
    return str


sp = lambda x: string.capwords(x.replace("_"," "))


def decode_inventory_data(raw):
    data = nbt.nbt.NBTFile(fileobj = io.BytesIO(base64.b64decode(raw)))
    #print(data.pretty_tree())
    for x in data['i']:
        #n is all the inventory data i guess
        n=[tag for tag in x.tags]
        if n:
            #n is a list, and n actually looks someting like
            #[<TAG_Short('id') at random stuff>, <TAG_Byte('Count') at random stuff>, <TAG_Compound('tag') at random stuff that idk>, <TAG_Short('Damage') at random stuff>]
            #we're trying to access the third tag, TAG_Compound('tag'), so we use n[2]
            dis = n[2]['display']
            #n[2] looks something like this: {TAG_Byte('Unbreakable'): 1, TAG_Int('HideFlags'): 254, TAG_Compound('display'): {3 Entries}, TAG_Compound('ExtraAttributes'): {10 Entries}}
            #It may differ for different helmets, but for now just bear with me.
            #Anyway, the display looks like
            #{TAG_List('Lore'): [??? TAG_String(s)], TAG_String('Name'): name of ur stuff duh}
            #We want the name of the item, so we print out dis['Name'].
            print(str(n[1])+'x',loreprocessor(str(dis['Name'])))
            if 'enchantments' in n[2]['ExtraAttributes']:
                print('Enchantments:')
                enchs = n[2]['ExtraAttributes']['enchantments']
                for i in enchs:
                    print(sp(i), enchs[i])
                print('\n')
        else:
            pass


def checkTimesPlay():
    number = 0
    if len(profiles) == 1:
        data = requests.get(f'https://api.hypixel.net/skyblock/profile?key={api}&profile={profiles[number]}').json()
        for e in data:
            try:
                data = requests.get(f'https://api.hypixel.net/skyblock/profile?key={api}&profile={profiles[number]}').json()
                times_play = data["profile"]['members'][uuid]["dungeons"]["dungeon_types"]["catacombs"]['times_played']['7']
                print(cute_name[number], "played f7", times_play, 'times')
                break
            except:
                print(cute_name[number], "haven't played dungeon yet")
                break


    else:
        data = requests.get(f'https://api.hypixel.net/skyblock/profile?key={api}&profile={profiles[number]}').json()
        for e in data:
            try:
                data = requests.get(f'https://api.hypixel.net/skyblock/profile?key={api}&profile={profiles[number]}').json()
                times_play = data["profile"]['members'][profiles[number]]["dungeons"]["dungeon_types"]["catacombs"]['times_played']['7']
                print(cute_name[number],"played f7", times_play, 'times')
                number += 1
            except:
                print(cute_name[number],"haven't played dungeon yet")
                number += 1


def checkFastestTime():
    number = 0
    data = requests.get(f'https://api.hypixel.net/skyblock/profile?key={api}&profile={profiles[number]}').json()
    if len(profiles) == 1:
        for g in data:
            try:
                data = requests.get(f'https://api.hypixel.net/skyblock/profile?key={api}&profile={profiles[number]}')\
                    .json()
                personalBest = data['profile']['members'][uuid]['dungeons']['dungeon_types']['catacombs']['fastest_time']['7']
                personalBest = personalBest/1000/60
                print((cute_name[number]),'''personal best is ''', personalBest)
                break
            except:
                print((cute_name[number]),"haven't beaten floor 7 yet.")
                break
    else:
        for g in data:
            try:
                data = requests.get(f'https://api.hypixel.net/skyblock/profile?key={api}&profile={profiles[number]}'
                                    ).json()
                personalBest = data['profile']['members'][profiles[number]]['dungeons']['dungeon_types']['catacombs']['fastest_time']['7']
                personalBest = personalBest/1000/60
                print((cute_name[number]),'''personal best is ''', personalBest)
                number += 1
            except:
                print((cute_name[number]),"haven't beaten floor 7 yet.")
                number += 1


#data = requests.get(f'https://api.hypixel.net/skyblock/profile?key={api}&profile={profiles[1]}').json()
#items = data['profiles']['members'][profiles[1]]['inv_contents']


def checkInventory():
    number = 0
    if len(profiles) == 1:
        try:
            data = requests.get(f'https://api.hypixel.net/skyblock/profiles?key={api}&uuid={uuid}').json()
            for f in data['profiles']:
                if 'inv_contents' in f['members'][uuid]:
                    items = f['members'][uuid]['inv_contents']['data']
                    decode_inventory_data(items)
                    break
        except KeyError:
            data = requests.get(f'https://api.hypixel.net/skyblock/profiles?key={api}&uuid={uuid}').json()
            for f in data['profiles']:
                if 'inv_contents' in f['members'][uuid]:
                    items = f['members'][uuid]['inv_contents']['data']
                    decode_inventory_data(items)
                    break

    else:
        data = requests.get(f'https://api.hypixel.net/skyblock/profiles?key={api}&uuid={uuid}').json()
        try:
            data = requests.get(f'https://api.hypixel.net/skyblock/profiles?key={api}&uuid={uuid}').json()
            for i in profiles[number]:
                for f in data['profiles']:
                    if 'inv_contents' in f['members'][uuid]:
                        items = f['members'][uuid]['inv_contents']['data']
                        decode_inventory_data(items)
                        number += 1
        except KeyError:
            number -= 1
            for f in data['profiles']:
                if 'inv_contents' in f['members'][profiles[number]]:
                    items = f['members'][profiles[number]]['inv_contents']['data']
                    decode_inventory_data(items)
                    number += 2

checkInventory()
checkFastestTime()
checkTimesPlay()
