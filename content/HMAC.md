 [[SVR_ietf]] [[IPSec]]
Sender : 
Message 
Hash(Message)
Encrypt(Hash(Message)). = HashMessageAuthCode
with shared key or my private key 

Receiver: 
Message+HMAC 

Message
Hash(Message)
Decrypt(HMAC) with shared key or public key of sender

Compare Has(Messsage) = Decrypt(HMAC)
Sender is verified
