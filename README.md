### <easy_login>

<설명>

```php
<?php
  //prevent brute force
  sleep(3);

  //secret
  $id='*****';
  $pw='*****';
  $FLAG='*****';

  $s = mysqli_connect("localhost","admin","Eb6SfGEoydig","hi");
  $sql = "select * from hiru where name='{$_POST['name']}' and password='{$_POST['pw']}'";
  $result = mysqli_query($s,$sql);
  $row = mysqli_fetch_array($result);

  //encode to security
  if(empty($row['name'])||empty($row['password']))
  {
    echo 'no empty';

  }
  else{
    $result = base_convert($id,36,10)+base_convert($pw,36,10)-23230976;
    if($result=='admin'&&$row['name']==='debug'&&strlen($row['password'])<=5)//todo : convert name to secret
    {
      echo $FLAG;
    }
    else {
      echo 'no flag';
    }
  }
?>

```
login.php의 소스코드 입니다.

$result 비교연산자가 느슨한 비교연산자입니다. 따라서 느슨한 비교연산자가 취약점이 있습니다.
convert name to secret 을보니 관리자가 name 에 해당하는 문자열을 감추는것을 깜빡한듯합니다.
소스코드를 보면 row['name']이 debug일때 flag를 얻게되어있습니다.
따라서 name 은 debug 가됩니다.

base_convert 함수를 이용해 result 변수가 admin이면 flag를 얻게됩니다. 하지만 base_convert함수는 10진수로 변환이되고 연산을 통해
타입이 integer가 됩니다. 

느슨한 비교연산자는 문자열==0 이 1이됩니다. 따라서 $result 값이 0이되는 password를 얻으면됩니다.

따라서 
```php 
base_convert($id,36,10)+{pw}-23230976 = 0 
```
이되어야하므로 계산식은 
```php
base_convert((base_convert($id,36,10)-23230976),10,36)
```
이런식으로 나오게됩니다.이때 id는 관리자의 실수로인해 debug 가되고 
```php 
base_convert((base_convert('debug',36,10)-23230976),10,36)
```
이런식이됩니다.

이값을 join.php로 가입하고
로그인 해보면 flag가 나오게 됩니다.

이값을 출력해본다면
```php
flag
```
가 나오게됩니다.
따라서 id : debug | pw : flag 가되며 로그인하면 flag를 얻을수있습니다.

<설명>
