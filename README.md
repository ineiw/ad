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
<설명>


<설명>

