# mailkomplet-swiftmailer 

An Swiftmailer Transport for MailKomplet.

Send mail through MailKomplet from your favorite PHP frameworks!

##### 1. Include this package in your project:

```bash
composer require trueapps/mailkomplet-swiftmailer
```
##### 2. Use the transport to send a message:

```php
<?php
//import the transport from the standard composer directory:
require_once('./vendor/autoload.php');

$transport = new \MailKomplet\Transport('<BASE_CRYPT>','<APIKEY>');
$mailer = new Swift_Mailer($transport);

//Instantiate the message you want to send.
$message = (new Swift_Message('Hello from MailKomplet!'))
  ->setFrom(['john@example.com' => 'John Doe'])
  ->setTo(['jane@example.com'])
  ->setBody('<b>A really important message from our partners.</b>', 'text/html')
  ->addPart('Another important message from our partners.','text/plain');

//Add some attachment data:
$attachmentData = 'Some attachment data.';
$attachment = new Swift_Attachment($attachmentData, 'my-file.txt', 'application/octet-stream');

$message->attach($attachment);

//Send the message!
$mailer->send($message);
?>
```

##### 3. Throw exceptions on MailKomplet api errors:

```php
$transport = new \MailKomplet\Transport('<BASE_CRYPT>','<APIKEY>');
$transport->registerPlugin(new \MailKomplet\ThrowExceptionOnFailurePlugin());

$message = new Swift_Message('Hello from mailKomplet!');
$mailer->send($message); // Exception is throw when response !== 200
```

##### 4. Use default headers:

You can set default headers at Transport-level, to be set on every message, unless overwritten.

```php
$defaultHeaders = ['X-MK-Tag' => 'my-tag'];

$transport = new \MailKomplet\Transport('<BASE_CRYPT>','<APIKEY>', $defaultHeaders);

$message = new Swift_Message('Hello from MailKomplet!');

// Overwriting default headers
$message->getHeaders()->addTextHeader('X-MK-Tag', 'custom-tag');
```

##### Notes:

- The Transport uses the [MailKomplet API](https://api.mail-komplet.cz) internally to send mail, via the /transactionalEmails endpoint.
