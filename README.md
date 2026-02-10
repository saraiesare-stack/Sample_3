"""
Message Counter Telegram Bot
----------------------------
Counts how many messages each user has sent.
"""

from telegram import Update
from telegram.ext import ApplicationBuilder, MessageHandler, ContextTypes, filters

TOKEN = "PUT_YOUR_BOT_TOKEN_HERE"

user_counts = {}


async def count_messages(update: Update, context: ContextTypes.DEFAULT_TYPE):
    """Increase and show message count for each user."""
    user_id = update.message.from_user.id

    user_counts[user_id] = user_counts.get(user_id, 0) + 1
    count = user_counts[user_id]

    await update.message.reply_text(f"You have sent {count} messages 📩")


def main():
    app = ApplicationBuilder().token(TOKEN).build()
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, count_messages))

    print("Counter bot is running...")
    app.run_polling()


if __name__ == "__main__":
    main()
