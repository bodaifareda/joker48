from telegram import Update
from telegram.ext import Application, CommandHandler, MessageHandler
from telegram.ext import filters
import logging

# Enable logging
logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
                    level=logging.INFO)
logger = logging.getLogger(__name__)

# Function to handle the /start command
async def start(update: Update, context):
    await update.message.reply_text('مرحبًا! أرسل لي معادلة رياضية وسأحسبها لك.')

# Function to evaluate mathematical expressions
async def calculate(update: Update, context):
    try:
        expression = update.message.text
        result = eval(expression)
        await update.message.reply_text(f'النتيجة: {result}')
    except Exception as e:
        await update.message.reply_text('عذراً، حدث خطأ أثناء الحساب. تأكد من صحة المعادلة.')

# Main function to run the bot
def main():
    # Create application instance
    application = Application.builder().token("7945125120:AAEBbEi-xHv3lJ2nmn4mt4jOe6Bhix0JlM0").build()
    
    # Adding handlers
    application.add_handler(CommandHandler("start", start))
    application.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, calculate))

    # Start the bot
    application.run_polling()

if __name__ == '__main__':
    main()
