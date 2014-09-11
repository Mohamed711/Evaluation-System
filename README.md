# Final Task

require 'money'
require 'money/bank/google_currency'



class Numeric


# set the seconds after than the current rates are automatically expired
# by default, they never expire
Money::Bank::GoogleCurrency.ttl_in_seconds = 86400

# set default bank to instance of GoogleCurrency
bank = Money::Bank::GoogleCurrency.new


@@currency = {dollar: 7.15 , euro: 9.26, egp: 1 , yen: 0.067}
@@currency_api = {dollar: "USD" , euro: "EUR" , egp: "EGP" , yen: "JPY" }
time=Time.new

# Get the value in egp directly
  @@currency_api.each do |currency, rate|
   define_method(currency) do
      self * (bank.get_rate(@@currency_api [currency].to_sym  , :egp )).to_f
    end

    alias_method "#{currency}s".to_sym, currency
 end


# add a new currency
def add_currency (new_currency={})
new_currency.each do |added_currency,rate|
	@@currency[added_currency]=rate
	end
end


# convert between currencies
def convert_currency (converted_currency={})

# set default bank to instance of GoogleCurrency
bank = Money::Bank::GoogleCurrency.new

	# store the currency required to change from and to it 
	currency_to = converted_currency[:to]
	currency_from = converted_currency[:from]

	# get the exhange rate values in both cases egp or egps

	unless @@currency[currency_to] 
	currency_to = (currency_to.to_s.chop).to_sym
	end
	
	unless @@currency[currency_from] 
	 currency_from = (currency_from.to_s.chop).to_sym
	end
	
	 puts self*(bank.get_rate(@@currency_api [currency_from].to_sym  ,  @@currency_api [currency_to].to_sym)).to_f
	end

end


#add_currency(saudi_arabia: 1.5)
20.convert_currency(from: :euros , to: :egps)
puts 100.dollars	
puts 378478.euros


