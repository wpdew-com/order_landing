# Order Processing Script

This PHP script handles order processing by capturing form data, validating it, and sending it to various CRMs and Telegram. The script also manages email notifications and checks for spam IPs.

## Features

- **Form Data Capture:** Collects order details from an HTML form.
- **Spam Detection:** Checks if the request comes from known spam IP addresses.
- **CRM Integration:** Sends order data to multiple CRMs (LP CRM, SalesDrive, KeyCRM, Ebash CRM).
- **Telegram Notification:** Sends order details to a specified Telegram chat.
- **Email Notification:** Sends order confirmation email.
- **ReCaptcha Verification:** (Commented out) Verifies the request using Google ReCaptcha.

## Requirements

- PHP 5.6 or higher
- cURL extension enabled
- A valid Telegram bot token and chat ID
- CRM API tokens and URLs

## Setup

1. **Configure CRMs and Telegram:**

   - Replace placeholder values for `$tgtoken`, `$tgchatid`, `$crm_lp_token`, `$crm_lp_adress`, `$crm_salesdrive_token`, `$crm_salesdrive_sources`, `$crm_key_token`, `$crm_key_sources`, `$crm_ebash_token`, `$crm_ebash_adress`, and `$crm_ebash_ofise` with your actual credentials.

2. **Spam IPs:**

   - Add known spam IP addresses to the `$spamip` array.

3. **ReCaptcha:**

   - Set your site key in the `$SiteKey` variable if using ReCaptcha (currently commented out).

4. **Form Fields:**

   - Ensure your HTML form includes fields for `name`, `phone`, and other optional fields like `fbp`, `comment`, `product_id`, etc.

## Usage

Include this script in your order processing form handler. The script will:

1. Capture and sanitize form data.
2. Check if the IP address is in the spam list.
3. Send order data to Telegram, configured CRMs, and via email.

### Example Form

html

Копировать код

`
<form action="order.php" method="post">
	<h3 class="mb-3">Заповніть, будь ласка, форму нижче</h3>
	<input class="form-control form-control-lg" type="text" placeholder="Ваше ім'я" name="name" required="">
	<input class="form-control form-control-lg mt-3" type="text" placeholder="Ваш номер телефону" name="phone" required="" />
	<select name="comment" class="form-control-lg form-select mt-3" required="">
		<option value="" selected disabled>Оберіть кількість</option>
		<option value="1 шт" data-productid="<?= $product_id; ?>" data-count="1" data-prise="<?= $price_new; ?>">1 шт - <?= $price_new; ?>грн</option>
		<option value="2 шт" data-productid="<?= $product_id; ?>" data-count="2" data-prise="<?= $price_new2; ?>">2 шт - <?= round($price_new2 *2); ?>грн (-5% знижки)</option>
		<option value="3 шт" data-productid="<?= $product_id; ?>" data-count="3" data-prise="<?= $price_new3; ?>">3 шт - <?= round($price_new3 *3); ?>грн (-10% знижки)</option>
		<option value="4 шт" data-productid="<?= $product_id; ?>" data-count="4" data-prise="<?= $price_new4; ?>">4 шт - <?= round($price_new4 *4); ?>грн (-15% знижки)</option>
	</select>
	<input type="hidden" name="product" id="hiddenProduct"  value="Quick landing page" hidden="hidden" />
	<input type="hidden" name="product_id" id="product_id" value="<?= $product_id; ?>" hidden="hidden" />
	<input type="hidden" name="product_price" id="product_price"  value="<?= $price_new; ?>" hidden="hidden" />
	<input type="hidden" name="fbp" id="fbp"  value="<?= $fbp; ?>" hidden="fbp" />
	<input type="hidden" name="count" value="1"/>
	<input type="hidden" name="servername" value="<?= dirname($_SERVER['SCRIPT_NAME']);?>">
	<input type="hidden" name="type" value="offer">
	<button type="submit" class="btn btn-danger btn-buy mt-4 mb-2">
		Зробити замовлення
	</button>
	<span class="mt-4 text-center" style="font-size: 12px">
    	Відправляючи дані ви погоджуєтесь з <br />
    	<a href="politics.html" target="_blank">політикою конфіденційності</a>
    </span>
</form>
`

## Code Overview

### Main Sections

1. **Initialization:**

   - Starts session and sets the timezone.
   - Defines spam IPs, CRM tokens, and other configurations.

2. **Form Data Capture:**

   - Retrieves and sanitizes form data.

3. **Spam Check:**

   - Checks if the user's IP address is in the spam list.

4. **Order Processing:**

   - Defines the `Order` class with methods to send data to Telegram, CRMs, and via email.
   - Depending on the form type, processes the order and sends data to the relevant services.

### Order Class Methods

- **sendToTelegram:** Sends order details to a Telegram chat.
- **sentdToLpCrm:** Sends order details to LP CRM.
- **sendEmail:** Sends an email with order details.
- **sentdToSalesDrive:** Sends order details to SalesDrive.
- **sentdToKeyCrm:** Sends order details to KeyCRM.
- **sentdEbashCrm:** Sends order details to Ebash CRM.
- **getCaptcha:** Verifies ReCaptcha response (currently not in use).

## Customization

- Update the CRM and email configurations as per your requirements.
- Modify the HTML form and PHP script to handle additional fields as needed.
- Uncomment and configure ReCaptcha verification if required.

## Security Considerations

- Ensure that the script is protected from SQL injection and cross-site scripting (XSS) attacks by validating and sanitizing all user inputs.
- Use HTTPS to encrypt data transmitted between the client and server.

## License

This script is open-source and available under the MIT License. Feel free to use and modify it as per your needs.
