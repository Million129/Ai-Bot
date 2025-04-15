
import telebot
import requests
from time import sleep

API_KEYS = [
    "key_66a7e03c96a55aaf9e6c4ec0c01fc975414023e41e8e13d6f916f39394778691432d89958acbb80a4e720c4b9c5fe797b6410049bf9b7879df88e0010c2d42f8",
    "key_33d4a98af36894b4145f7ba2f2feeed1b0ebc68bb684905b917f065924c7cb4b47ed3318c1837453d7c569e650ae1c058539a136993e56156ac49af884d9d808",
    "key_db708795bb91d29dcd67997d577a0db43291e36cfce545e60e2892fb386ac1ef956839418dd028d46a58962297174cf68f56e5ecd151a5476134633b353ee73b",
    "key_822ffdc4e77fc90058c4777588b2078fb619791bef936bb9e3c3502bad881b1ad3750a2f3c3fe264c312c65e447670a5b54130e25fb38bb1a0953d2efa995ee3",
    "key_0c40cf3fdc3c9f6449de3012379beb675f00c9bb9ac474e0295c221e5c0a8ca2067b66c10169c26955dd35c7070bb38ad2647d68dc73ebfac826d218e87dce19",
    "g6OnFluavLFEUMwN9A3j76cHnHbjEDBInSUn7ENY"
]

bot = telebot.TeleBot("7818768074:AAFIdqY2fVh0GCLgPpa6BTX7cA1nLyfMh-g")
user_data = {}

@bot.message_handler(commands=['start'])
def send_welcome(message):
    cid = message.chat.id
    user_data[cid] = {}
    bot.send_message(cid, "أهلاً بيك في بوت إنشاء الفيديوهات بالذكاء الاصطناعي! اختار نوع الفيديو:", reply_markup=main_menu())

def main_menu():
    markup = telebot.types.InlineKeyboardMarkup()
    types = ["واقعي", "كرتون", "رسم يدوي", "أنمي", "3D", "سينمائي"]
    for t in types:
        markup.add(telebot.types.InlineKeyboardButton(text=t, callback_data=f"type:{t}"))
    return markup

@bot.callback_query_handler(func=lambda call: True)
def handle_callback(call):
    cid = call.message.chat.id
    data = call.data

    if cid not in user_data:
        user_data[cid] = {}

    if data.startswith("type:"):
        user_data[cid]["type"] = data.split(":")[1]
        markup = telebot.types.InlineKeyboardMarkup()
        ages = ["صغار", "متوسط", "كبار"]
        for age in ages:
            markup.add(telebot.types.InlineKeyboardButton(text=age, callback_data=f"age:{age}"))
        bot.edit_message_text("اختر الفئة العمرية:", cid, call.message.message_id, reply_markup=markup)

    elif data.startswith("age:"):
        user_data[cid]["age"] = data.split(":")[1]
        markup = telebot.types.InlineKeyboardMarkup()
        speakers = ["رجل", "امرأة", "طفل"]
        for s in speakers:
            markup.add(telebot.types.InlineKeyboardButton(text=s, callback_data=f"speaker:{s}"))
        bot.edit_message_text("اختر نوع المتحدث:", cid, call.message.message_id, reply_markup=markup)

    elif data.startswith("speaker:"):
        user_data[cid]["speaker"] = data.split(":")[1]
        markup = telebot.types.InlineKeyboardMarkup()
        langs = ["عربي", "إنجليزي"]
        for lang in langs:
            markup.add(telebot.types.InlineKeyboardButton(text=lang, callback_data=f"lang:{lang}"))
        bot.edit_message_text("اختر اللغة:", cid, call.message.message_id, reply_markup=markup)

    elif data.startswith("lang:"):
        user_data[cid]["lang"] = data.split(":")[1]
        markup = telebot.types.InlineKeyboardMarkup()
        with_text = ["بنص", "بدون نص"]
        for w in with_text:
            markup.add(telebot.types.InlineKeyboardButton(text=w, callback_data=f"text:{w}"))
        bot.edit_message_text("هل تريد تضمين نص في الفيديو؟", cid, call.message.message_id, reply_markup=markup)

    elif data.startswith("text:"):
        user_data[cid]["text"] = data.split(":")[1]
        markup = telebot.types.InlineKeyboardMarkup()
        durations = ["30 ثانية", "1 دقيقة", "3 دقائق", "5 دقائق", "10 دقائق"]
        for d in durations:
            markup.add(telebot.types.InlineKeyboardButton(text=d, callback_data=f"duration:{d}"))
        bot.edit_message_text("اختر مدة الفيديو:", cid, call.message.message_id, reply_markup=markup)

    elif data.startswith("duration:"):
        user_data[cid]["duration"] = data.split(":")[1]
        bot.send_message(cid, "جاري تنفيذ الفيديو...")

        for key in API_KEYS:
            try:
                # ده مجرد تمثيل لاستخدام API
                r = requests.get("https://example.com/api", headers={"Authorization": key}, timeout=10)
                if r.status_code == 200:
                    bot.send_message(cid, "تم إنشاء الفيديو بنجاح!")
                    break
            except Exception as e:
                continue
        else:
            bot.send_message(cid, "فشل في إنشاء الفيديو، جرب وقت لاحق.")

bot.infinity_polling()
