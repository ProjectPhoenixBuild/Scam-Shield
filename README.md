import logging
from telegram import Update
from telegram.ext import Application, CommandHandler, ContextTypes

logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', level=logging.INFO)

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text('Welcome to 1-Hour Income. Use /teach to start earning in 60 minutes.')

async def teach(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text('What skill can you teach in 1 hour? Reply with: /skill [your skill]')

def main():
    application = Application.builder().token('YOUR_BOT_TOKEN').build()
    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("teach", teach))
    application.run_polling()

if __name__ == '__main__':
    main()
