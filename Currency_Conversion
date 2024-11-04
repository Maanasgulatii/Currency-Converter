from forex_python.converter import CurrencyRates
import json
from datetime import datetime
import requests
from requests.exceptions import RequestException

class CurrencyConverter:
    def __init__(self):
        self.currency_rates = CurrencyRates()
        # Extended list of major world currencies with approximate rates
        self.backup_currencies = {
            'USD': 1.0,      # US Dollar
            'EUR': 0.85,     # Euro
            'GBP': 0.73,     # British Pound
            'JPY': 110.0,    # Japanese Yen
            'AUD': 1.35,     # Australian Dollar
            'CAD': 1.25,     # Canadian Dollar
            'CHF': 0.92,     # Swiss Franc
            'CNY': 6.45,     # Chinese Yuan
            'INR': 74.5,     # Indian Rupee
            'NZD': 1.42,     # New Zealand Dollar
            'SGD': 1.35,     # Singapore Dollar
            'HKD': 7.78,     # Hong Kong Dollar
            'SEK': 8.65,     # Swedish Krona
            'KRW': 1175.0,   # South Korean Won
            'BRL': 5.25,     # Brazilian Real
            'RUB': 73.5,     # Russian Ruble
            'ZAR': 14.8,     # South African Rand
            'MXN': 20.0,     # Mexican Peso
            'AED': 3.67,     # UAE Dirham
            'THB': 33.3,     # Thai Baht
            'TRY': 8.65,     # Turkish Lira
            'SAR': 3.75,     # Saudi Riyal
            'DKK': 6.35,     # Danish Krone
            'NOK': 8.95,     # Norwegian Krone
            'ILS': 3.25,     # Israeli Shekel
            'MYR': 4.20,     # Malaysian Ringgit
            'PLN': 3.90,     # Polish Złoty
            'PHP': 50.5      # Philippine Peso
        }

    def convert_currency(self, amount, from_currency, to_currency):
        try:
            # Try to use live rates first
            return self.currency_rates.convert(from_currency, to_currency, amount)
        except Exception as e:
            # Fallback to backup conversion if live rates fail
            try:
                if from_currency in self.backup_currencies and to_currency in self.backup_currencies:
                    # Convert to USD first (as base), then to target currency
                    usd_rate_from = self.backup_currencies[from_currency]
                    usd_rate_to = self.backup_currencies[to_currency]
                    
                    # Convert to USD first
                    usd_amount = amount / usd_rate_from
                    # Then convert from USD to target currency
                    final_amount = usd_amount * usd_rate_to
                    
                    print("Note: Using backup exchange rates as live rates are unavailable.")
                    return final_amount
                else:
                    return f"Error: Currency not supported or service unavailable."
            except Exception as e:
                return f"Error during conversion: {str(e)}"
    
    def list_supported_currencies(self):
        try:
            # Try to get live rates first
            live_rates = self.currency_rates.get_rates('EUR')  # Using EUR as base for variety
            return live_rates
        except Exception:
            # Return backup currencies if live rates are unavailable
            print("Note: Using backup currency list as live rates are unavailable.")
            
            # Convert backup rates to show them relative to EUR instead of USD
            eur_rate = self.backup_currencies['EUR']
            relative_rates = {}
            for currency, rate in self.backup_currencies.items():
                relative_rates[currency] = rate / eur_rate
            
            return relative_rates

def main():
    converter = CurrencyConverter()
    print("Welcome to Currency Converter")
    print("Supports major currencies worldwide")
    
    while True:
        print("\nMenu:")
        print("1. Convert Currency")
        print("2. List Supported Currencies")
        print("3. Exit")
        
        choice = input("Enter your choice (1/2/3): ")
        
        if choice == "1":
            try:
                amount = float(input("Enter the amount to convert: "))
                if amount <= 0:
                    print("Please enter a positive amount.")
                    continue
                    
                print("\nAvailable currencies:", ", ".join(sorted(converter.backup_currencies.keys())))
                from_currency = input("Enter the currency to convert from (e.g., USD, EUR): ").upper()
                to_currency = input("Enter the currency to convert to (e.g., USD, EUR): ").upper()
                
                if from_currency not in converter.backup_currencies:
                    print(f"Error: {from_currency} is not a supported currency.")
                    continue
                    
                if to_currency not in converter.backup_currencies:
                    print(f"Error: {to_currency} is not a supported currency.")
                    continue
                
                converted_amount = converter.convert_currency(amount, from_currency, to_currency)
                if isinstance(converted_amount, float):
                    print(f"\n{amount:,.2f} {from_currency} = {converted_amount:,.2f} {to_currency}")
                else:
                    print(converted_amount)
                    
            except ValueError:
                print("Please enter a valid number for the amount.")
            except Exception as e:
                print(f"An error occurred: {str(e)}")
                
        elif choice == "2":
            supported_currencies = converter.list_supported_currencies()
            if isinstance(supported_currencies, dict):
                print("\nSupported currencies and their current rates:")
                base_currency = "EUR"
                print(f"(Base Currency: {base_currency})")
                print("\nCurrency Code | Currency Name")
                print("-" * 40)
                
                # Dictionary of currency names
                currency_names = {
                    'USD': 'US Dollar',
                    'EUR': 'Euro',
                    'GBP': 'British Pound',
                    'JPY': 'Japanese Yen',
                    'AUD': 'Australian Dollar',
                    'CAD': 'Canadian Dollar',
                    'CHF': 'Swiss Franc',
                    'CNY': 'Chinese Yuan',
                    'INR': 'Indian Rupee',
                    'NZD': 'New Zealand Dollar',
                    'SGD': 'Singapore Dollar',
                    'HKD': 'Hong Kong Dollar',
                    'SEK': 'Swedish Krona',
                    'KRW': 'South Korean Won',
                    'BRL': 'Brazilian Real',
                    'RUB': 'Russian Ruble',
                    'ZAR': 'South African Rand',
                    'MXN': 'Mexican Peso',
                    'AED': 'UAE Dirham',
                    'THB': 'Thai Baht',
                    'TRY': 'Turkish Lira',
                    'SAR': 'Saudi Riyal',
                    'DKK': 'Danish Krone',
                    'NOK': 'Norwegian Krone',
                    'ILS': 'Israeli Shekel',
                    'MYR': 'Malaysian Ringgit',
                    'PLN': 'Polish Złoty',
                    'PHP': 'Philippine Peso'
                }
                
                for currency in sorted(supported_currencies.keys()):
                    currency_name = currency_names.get(currency, 'Unknown Currency')
                    rate = supported_currencies[currency]
                    print(f"{currency:3} | {currency_name:20} | Rate: {rate:.4f}")
            else:
                print(supported_currencies)
                
        elif choice == "3":
            print("Thank you for using Currency Converter by Maanas. See you!")
            break
            
        else:
            print("Invalid choice. Please enter 1, 2, or 3.")

if __name__ == "__main__":
    main() 
