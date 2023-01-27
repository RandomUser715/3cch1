import telebot
import random
import datetime
import os
import tkinter as tk
import tkinter.filedialog as fd

folder = ("/home/paladin/PycharmProjects/pythonProject/Material/3cch1/DB")
roadtxt = open("roads.txt", "a")
files = os.listdir(folder)
listroads = []

while True:
    line = roadtxt.readline()
    listroads.append(line.strip())
    if not line:
        listroads.pop(-1)
        break
roadtxt.close()

i = 0
while True:
    if i < len(files):
        files[i] = folder + '/' + files[i]
        i += 1
    else:
        break

result = list(set(listroads).difference(files)) + list(set(files).difference(listroads))
roadtxt = open("roads.txt", "a")

while True:
    if result:
        roadtxt.write(result[-1] + '\n')
        result.pop(-1)
    else:
        roadtxt.close()
        break

file = open("roads.txt", "r")
listroads = []

while True:
    line = file.readline()
    listroads.append(line.strip())
    if not line:
        listroads.pop(-1)
        amount = len(listroads)
        amount -= 1
        file.close()
        break

bot = telebot.TeleBot('5954989987:AAHEg2qOtqvQvydPG2InRowwZW8qaV2IIUI')


@bot.message_handler(commands=['start', 'help'])
def start_message(message):
    bot.send_message(message.chat.id, "Привет! Это бот рандомных изображений. "
                                      "Напишите /img для работы.")


@bot.message_handler(commands=['img'])
def random_img(message):
    index = random.randint(0, amount)
    now = datetime.datetime.now()
    print(listroads[index], "Time:", now)
    photo = open(listroads[index], 'rb')
    bot.send_photo(message.chat.id, photo)
    photo.close()


@bot.message_handler(content_types=['text'])
def get_text_messages(message):
    if message.text != "Привет":
        bot.send_message(message.from_user.id, "Неизвестная комманда. Напиши /help.")



bot.infinity_polling()
