from telegram import InlineKeyboardButton, InlineKeyboardMarkup, Update
from telegram.ext import Updater, CommandHandler, CallbackQueryHandler, CallbackContext

# Define your token (replace 'YOUR_TOKEN' with your actual bot token)
TOKEN = 'YOUR_TOKEN'

# Function to start the bot
def start(update: Update, context: CallbackContext) -> None:
    keyboard = [
        [InlineKeyboardButton("Option 1", callback_data='option1')],
        [InlineKeyboardButton("Option 2", callback_data='option2')]
    ]
    reply_markup = InlineKeyboardMarkup(keyboard)
    update.message.reply_text('Choose an option:', reply_markup=reply_markup)

# Function to handle button clicks
def button(update: Update, context: CallbackContext) -> None:
    query = update.callback_query
    query.answer()
    choice = query.data

    if choice == 'option1':
        query.edit_message_text(text="You chose Option 1!")
    elif choice == 'option2':
        query.edit_message_text(text="You chose Option 2!")

# Main function to run the bot
def main() -> None:
    updater = Updater(TOKEN, use_context=True)
    dispatcher = updater.dispatcher

    # Add handlers
    dispatcher.add_handler(CommandHandler('start', start))
    dispatcher.add_handler(CallbackQueryHandler(button))

    # Start the bot
    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()
