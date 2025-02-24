from telegram import Update, InputFile from telegram.ext import Updater, CommandHandler, MessageHandler, Filters, CallbackContext import requests import os

def download_instagram_video(url): api_url = f"https://api.instagram-downloader.com/?url={url}" response = requests.get(api_url) if response.status_code == 200: return response.json().get("video_url") return None

def start(update: Update, context: CallbackContext): update.message.reply_text("Assalomu alaykum! Instagram videolarini yuklab olish uchun menga link yuboring.")

def handle_message(update: Update, context: CallbackContext): url = update.message.text video_url = download_instagram_video(url) if video_url: video_response = requests.get(video_url) with open("video.mp4", "wb") as f: f.write(video_response.content) update.message.reply_video(video=open("video.mp4", "rb")) os.remove("video.mp4") else: update.message.reply_text("Kechirasiz, videoni yuklab bo‘lmadi. Iltimos, boshqa link sinab ko‘ring.")

def main(): TOKEN = "7582233024:AAH-UwDFk06U0debeovAdcu-E9Vpr2pDb30" updater = Updater(TOKEN, use_context=True) dp = updater.dispatcher

dp.add_handler(CommandHandler("start", start))
dp.add_handler(MessageHandler(Filters.text & ~Filters.command, handle_message))

updater.start_polling()
updater.idle()

if name == "main": main()

