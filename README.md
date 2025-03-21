import os
   import requests
   from telegram import Update
   from telegram.ext import Updater, CommandHandler, MessageHandler, Filters, CallbackContext

   # Configurações
   TELEGRAM_TOKEN = "7636047693:AAFnP7BfCxGKAi-E1M7OSXolQEcoiUsVYlo"
   DEEPSEEK_API_KEY = 'jina_1fd80b89346d4a45b2f6c0d801aada2fQIsefw7r4mYbKmY-dnoftJb6Idiv'
   DEEPSEEK_API_URL = 'https://api.deepseek.com/v1/chat/completions'  # Exemplo de endpoint

   # Função para enviar mensagem para a API do DeepSeek
   def send_to_deepseek(message):
       headers = {
           'Authorization': f'Bearer {DEEPSEEK_API_KEY}',
           'Content-Type': 'application/json'
       }
       data = {
           'prompt': message,
           'max_tokens': 150
       }
       response = requests.post(DEEPSEEK_API_URL, headers=headers, json=data)
       return response.json().get('choices', [{}])[0].get('text', 'Desculpe, não entendi.')

   # Comando /start
   def start(update: Update, context: CallbackContext):
       update.message.reply_text('Olá! Eu sou um bot integrado com o DeepSeek. Como posso ajudar?')

   # Resposta a mensagens
   def handle_message(update: Update, context: CallbackContext):
       user_message = update.message.text
       response = send_to_deepseek(user_message)
       update.message.reply_text(response)

   # Inicialização do bot
   def main():
       updater = Updater(7636047693:AAFnP7BfCxGKAi-E1M7OSXolQEcoiUsVYlo)
       dispatcher = updater.dispatcher

       dispatcher.add_handler(CommandHandler("start", start))
       dispatcher.add_handler(MessageHandler(Filters.text & ~Filters.command, handle_message))

       updater.start_polling()
       updater.idle()

   if __name__ == '__main__':
       main()