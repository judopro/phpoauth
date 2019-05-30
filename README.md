# phpoauth
PHPOAUTH helps you quickly program OAuth capable applications in PHP. 

You can create various supported Oauth signatures (Plaintext, SHA1, SHA256) with ease and do more. 
We use this intensively for Netsuite integration with different 3rd party platforms. 

Recently added SHA256 support as it is the commonly accepted standard now, since Plaintext, and SHA1 are now obsolete.

# Example using SHA256
```
$url_to_request = "INSERT_URL";
$app_token_consumer_key = "INSERT_CONSUMER_KEY";
$app_token_consumer_secret = "INSERT_CONSUMER_SECRET";
$user_token_id = "INSERT_USER_TOKEN";
$user_token_secret = "INSERT_USER_TOKEN_SECRET";

$nonce = mt_rand();
$token = new OAuthToken($user_token_id, $user_token_secret);
$consumer = new OAuthConsumer($app_token_consumer_key, $app_token_consumer_secret);
        
$sig = new OAuthSignatureMethod_HMAC_SHA256();

$params = array(
        'oauth_nonce' => $nonce,
        'oauth_timestamp' => idate('U'),
        'oauth_version' => '1.0',
        'oauth_token' => $user_token_id,
        'oauth_consumer_key' => $app_token_consumer_key,
        'oauth_signature_method' => $sig->get_name()
);

$req = new OAuthRequest('GET', $url_to_request, $params);
$req->set_parameter('oauth_signature', $req->build_signature($sig, $consumer, $token));
```

