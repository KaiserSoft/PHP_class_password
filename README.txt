class_password.php
http://www.kaisersoft.net/t.php?cpasswd

This is a PHP class for PHP developers to quickly create secure password hashes. Use this to ensure that a compromised database does not reveal passwords.
 The class will attempt to generate a 10000 round sha512 hash using crypt(). It will fallback to a 18000 round md5 hash if crypt is not available.


Getting a hash from the calss is almost as simple as calling md5();
	require_once 'class_password.php';
	$pwd = new password();
	$hash = $pwd->hash('StringToBeHashed'); //hash the password



At this point $hash will contain one of the following
	//default sha512
	$hash = $pwd->hash('StringToBeHashed'); //hash the password
	var_dump($hash);
	//string '$6$rounds=10000$QkwwE8U6BhPNhTkw$WCTJ/.Bv6w7shcvcSEIKy652QOAl4CX1wScY31Z1KJEJImrpQbtnbPcyqR6XtiZ3jVTHEl15Zg1X94l0ofNLq1' (length=119)

	//sha256
	$pwd->set_hash_type('sha256'); //force hash type
	$hash = $pwd->hash('StringToBeHashed'); //hash the password
	var_dump($hash);
	//string '$5$rounds=10000$ppMqHDNZJr/XuL28$NEESidNVTJ3BtmEUFc0IcaD6vQxys091tfH5WUgO2z0' (length=76)

	//fallback md5
	$pwd->set_hash_type('md5'); //force hash type
	$hash = $pwd->hash('StringToBeHashed'); //hash the password
	var_dump($hash);
	//string '$CL$6000$9c3f519be0dfa110a47ba79a11bd4550$6132c803417e061d7e3b48103402d812' (length=74)



Store $hash in your database and use it like this to revalidate a password.
	require_once 'class_password.php';
	$pwd = new password();
	//fill $db_hash with a database query
	$check = 'SomeStringToValidate';
	if( $pwd->validate($db_hash, $check) )
	    echo 'valid';



The class has the following methods

/**
 * function responsible for hashing strings securely
 * @param string $string Password to be hashed
 * @param string $salt=null Optional salt, uses rand_string() if null
 * @return string/bol hashed string OR boolean false on failure.
 */
function hash( &$string, $pass_the_salt=null, $use_crypt=true)

/**
 * function to compare a string to a previously hashed string
 * @param string &$hash previously hashed string
 * @param string &$password string to validate
 * @return bool true if the password generates the same hash OR false if they don't match 
 */
function validate( &$hash, &$password )

/**
 * function to rehash a previously hashed string. 
 * @param string &$hash previously hashed string
 * @param string &$password string to validate
 * @return string/bool hashed password or bool false on failure
 */
function re_hash( &$hash, &$password )

/**
 * function to specify how many rounds the hash function specified by $type will use
 * @param string $type hash type to change sha512, sha256 or md5
 * @param int $rounds rounds to run
 * @return bool true/false true on success, false on error 
 */
function set_hash_rounds( $type, $rounds )

/**
 * function to change the hash type
 * @param string $type Possible values sha512, sha256 or md5
 * @return bool true/false true on change, false on error
 */
function set_hash_type( $type )

/**
 * create a random string, uses mt_rand. 
 * rand_string(20, array('A','Z','a','z',0,9), '`,~,!,@,#,%,^,&,*,(,),_,|,+,=,-');
 * rand_string(16, array('A','Z','a','z',0,9), '.,/')
 * @param integer $lenth length of random string
 * @param array $range specify range as array array('A','Z','a','z',0,9) == [A-Za-z0-9]
 * @param string $other comma separated list of characters !,@,#,$,%,^,&,*,(,)
 * @return string random string of requested length
 */
function rand_string($lenth, $range=array('A','Z','a','z',0,9), $other='' )