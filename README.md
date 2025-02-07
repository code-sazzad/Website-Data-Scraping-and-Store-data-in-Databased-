# Website-Data-Scraping-and-Store-data-in-Databased-
Website Data Scraping and Store data in Database and create Your own API

        <?php
        
         $hostname= 'localhost';
         $user_name= 'bongote1_chat_app';
         $password= 'sazzadSA017#';
         $dbname='bongote1_chatapp';
         
         
         $con= mysqli_connect($hostname,$user_name,$password,$dbname);
        
         $html_code=file_get_contents('https://bongotechbd.xyz/crypto/simple_crypto.html');
         
         libxml_use_internal_errors(true);
         $dom=new DOMDocument;
         $dom-> loadHTML($html_code);
         
         $xpath=new DOMXpath($dom);
         $all_datas= $xpath-> query("//div[contains(@class, 'scroll-item')]//a");
        
         foreach( $all_datas as $all_data ){
        	 
        	 $text = $all_data-> textContent;
        	 $cleantext= preg_replace('/[^\x20-\x7E]/',' ',$text);
        	 $cleantext= preg_replace('/[\p{Zs}]+/u',' ', $cleantext);
        	 $cleantext=trim($cleantext);
        	 echo $cleantext ."<br>";
        	 
        	 
        	 $parts= explode(' ', $cleantext);
        	 
        	 if(count($parts)>=4){
        		 
        		 $name= $parts[0];
        		  $price= $parts[1];
        		   $price_change= $parts[2];
        		    $persent= $parts[3];
        		 
        		 	 $sql=" SELECT * FROM dse_company WHERE name LIKE '$name' ";
        		     $result= mysqli_query($con, $sql);
        		     $count= mysqli_num_rows($result);
        		    
        		    if($count>=1){
        		      $sql2= " UPDATE dse_company SET price= '$price', price_change= '$price_change' , persent= '$persent' WHERE name LIKE '$name'  ";
        		      mysqli_query($con,$sql2);
        			echo 'update';
        		}else{
        			
        			$sql2= " INSERT INTO dse_company (name,price,price_change,persent) VALUES ('$name','$price','$price_change','$persent') ";
        			mysqli_query($con,$sql2);
        			echo 'insert';
        		}
        		
        		
        
        	 }
        	 
        echo "<br><br>";
         }
        
        
        
        ?>
