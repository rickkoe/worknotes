# IBM GKLM

## Documentation Links

## Procedures
### Migrating Keys Using CLI
On SKLM server2, do the following:

1. Login as LOCAL ADMINISTRATOR
2. Change directories:
    
        cd \Program Files\IBM\WebSphere\AppServer\bin>

3. Log into the WebSphere CLI
	
        wsadmin.bat -username SKLMAdmin -password ESI@2019bpic -lang jython

4. Create an SSL certificate, say sslCert, on server2 if not previously done:
	
        print AdminTask.tklmCertCreate ('[-type selfsigned -alias sslCert -cn SSLCert -keyStoreName defaultKeyStore -usage SSLSERVER -validity 9000]')
	    Where sslCert is an example for an alias label.

5. List the certificate to verify it is created or if it is already there.
	
        print AdminTask.tklmCertList()
		
        Example Output clipped as we care only about the following 3 parameters:
		CTGKM0001I Command succeeded.
		...
		uuid = CERTIFICATE-xxxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxxxx
		alias(es) = sslCert
		key store name(s) = defaultKeyStore
		...
		
        Make a note of above 3 parameters as they will be used in the certificate export command:

6. Export sslCert into a file, say sslCert.der.
	    
        print AdminTask.tklmCertExport ('[-uuid CERTIFICATE-b4741a9-651e699c-8825-4f55-bed6-a37067bed6a0 -format DER -fileName sslCert.der]')
		
        CTGKM0001I Command succeeded.
		C:\Program Files\IBM\WebSphere\AppServer\products\sklm\data\sslCert.der

7. Copy sslCert.der file to TKLM server1.

On TKLM server1, do the following:

1. Login as LOCAL ADMINISTRATOR
2. Change directories:
    
        cd C:\IBM\tivoli\tiptklmV2\bin

3. Log into the Websphere CLI
    
        wsadmin.bat -username TKLMAdmin -password <put.pwd.here> -lang jython

4. List the certificates to get the 'keystore name' on that server1 as it will be used in the import certificate command:
        
        print AdminTask.tklmCertList()
            Example Output clipped as we care only about the following parameter:
            CTGKM0001I Command succeeded.
            ...
            key store name(s) = defaultKeyStore
            ...

5. Import sslCert.der on TKLM server1 that came from SKLM server2
        
        print AdminTask.tklmCertImport ('[-fileName C:\\SKLM\\svc-v7k_v5k.der -alias svc-v7k_v5k -format DER -keyStoreName "defaultKeyStore" -usage SSLSERVER]')

6. List the certificate to verify it is imported.
    
        print AdminTask.tklmCertList()
            Make a note of the following parameters as they will be used in the key export command:
            uuid = CERTIFICATE-xxxc20f3
            alias(es) = sslCert
            key store name(s) = Tivoli Key Lifecycle Manager Keystore

7. List the keys on TKLM server1 to find out which keys to move to SKLM server2:
    
        print AdminTask.tklmKeyList()

8. Export desired key or a range of keys into a file, say exportedKeys, while specifying the SSL certificate alias sslCert.der. Keys should be wrapped by the certificate.
        
        print AdminTask.tklmKeyExport('[-aliasRange uni00-c7 -fileName C:\\exportedKeys -keyStoreName "Tivoli Key Lifecycle Manager Keystore" -type secretkey -keyAlias sslCert]')

9. Copy exportedKeys file to SKLM server2.

On SKLM server2, do the following:

1. Import exportedKeys on SKLM server2 that came from TKLM server1
    
        print AdminTask.tklmKeyImport ('[-keyAlias sslCert -type secretkey -fileName C:\\exportedKeys -keyStoreName defaultKeyStore -usage LTO]')

2. List the keys to verify they are imported.
        
        print AdminTask.tklmKeyList()

3. Create a keygroup on SKLM server2, example name: tklmKeyGroup.
        
        print AdminTask.tklmGroupCreate ('[-name tklmKeyGroup -type keygroup -usage LTO]')

4. List the keygroup to either obtain the name if it was already created or confirm creation:
    
        print AdminTask.tklmGroupList('[-type keygroup -v y]')

5. Add one key into keygroup. 
    
        print AdminTask.tklmGroupEntryAdd('[-name tklmKeyGroup -type keygroup -entry "{type key} {alias aaa000000000000000019} {keyStoreName defaultKeyStore}"]')

6. Restart sklm.

