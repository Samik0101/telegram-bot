import telebot
from telebot import types
import random
import time
import threading

# ——— KONFIGURATSIYA ———
TOKEN         = '7746162082:AAHLYTP6PXlVcZ1GBeIOHpMgqUHUtvDj8ls'
PASSWORDS     = ['Samik_1923.', 'Samandar20061917','Samik_1917.']
CHANNEL_LINK  = 'https://t.me/+0lMkuMQvxpg1MTFi'

bot = telebot.TeleBot(TOKEN)

# Xato xabarlar
ERROR_MESSAGES = [
    "Yo‘q sen emassan bo`la olmaysan ham  !", "Ket bu yerdan yo`qol!", "Sen Samandar emassan!", "Xato vaqtingni bekorga ketkazyabsan!",
    "Botdan chiq Samandar bo`la olmaysan !", "Bekorchi vaqtingni rejasi yoq axmoq !", 
    "Vaqting kam yaxshi ishlar qil !", "Qayta urinib ko‘rma baribir foydasi yoq !"
]

# Asosiy menyu
def main_menu():
    kb = types.ReplyKeyboardMarkup(resize_keyboard=True, one_time_keyboard=True)
    kb.add("🔐 Xotiralar diyori")
    return kb

# Xabarni o'chirish funksiyasi
def safe_delete(chat_id, message_id):
    try:
        bot.delete_message(chat_id, message_id)
    except:
        pass

# /start komandasi
@bot.message_handler(commands=['start'])
def start_handler(message):
    sent = bot.send_message(
        message.chat.id,
        "👋 Salom! Kimsiz sizga nima kerak Sizga kim kerak Siz Samandar bo`lmasangiz men siz uchun ishlay olmayman buning uchun uzur sorayman :",
        reply_markup=main_menu()
    )
    threading.Timer(60.0, lambda: safe_delete(message.chat.id, sent.message_id)).start()

# Kanalga kirish uchun parol so'rash
@bot.message_handler(func=lambda m: m.text == "🔐 Xotiralar diyori")
def ask_password(message):
    # "🔐 Xotiralar diyori" so‘zini o‘chirish
    try:
        bot.delete_message(message.chat.id, message.message_id)
    except:
        pass

    msg = bot.send_message(
        message.chat.id,
        "🔑 Eslang:",
        reply_markup=types.ReplyKeyboardRemove()
    )
    threading.Timer(60.0, lambda: safe_delete(message.chat.id, msg.message_id)).start()
    bot.register_next_step_handler(msg, check_password)

# Parolni tekshirish
def check_password(message):
    chat_id = message.chat.id
    pwd = message.text.strip()

    try:
        bot.delete_message(chat_id, message.message_id)
    except:
        pass

    if pwd in PASSWORDS:
        sent = bot.send_message(
            chat_id,
            f"📢 Samandar Sizni uzoq kutdik sizni sog`indik sizni barcha xotiralaringiz shu yerda : \n{CHANNEL_LINK}",
            reply_markup=main_menu()
        )
        threading.Timer(60.0, lambda: safe_delete(chat_id, sent.message_id)).start()
    else:
        cnt = random.randint(3, 5)
        sample = random.sample(ERROR_MESSAGES, cnt)
        for err in sample:
            bot.send_chat_action(chat_id, 'typing')
            time.sleep(0.5)
            err_msg = bot.send_message(chat_id, err)
            threading.Timer(60.0, lambda m_id=err_msg.message_id: safe_delete(chat_id, m_id)).start()

        menu_msg = bot.send_message(
            chat_id,
            "❗ Siz u emassiz Siz Samandar emassiz sizni tanimayman :",
            reply_markup=main_menu()
        )
        threading.Timer(60.0, lambda: safe_delete(chat_id, menu_msg.message_id)).start()

if __name__ == '__main__':
    bot.infinity_polling()
