# Encoder_TSSOP14
The Encoder Converter board of Kicad Files for AMS AS5048A, AS5047D and others with SPI interface. 

[AMS社製磁気エンコーダ][0]　AS5048A, AS5047D等のTSSOP14を2.54mmピッチへ変換するKicadファイルです。  
3.3V電源及びSPIインタフェースのピンが出ています。  
他のピンは基板上のパット及びチップ抵抗のパターンからアクセス可能です。

<img src="https://github.com/meerstern/Encoder_TSSOP14/blob/master/encoder1.png" width="360">

<img src="https://github.com/meerstern/Encoder_TSSOP14/blob/master/encoder2.png" width="360">


## AS5048A/AS5047D仕様
  * 14bit(16384)/1回転の高分解能磁気エンコーダ変換基板です  
  * 磁石の向きをSPI通信を介して位置を読み取ることができます  
  * サンプリング周期は最大12kHzで値が更新されます  
  * ネオジム磁石とチップは5mm程度の距離でもある程度正確に読込できます  
  * 磁石中心とチップ中心を高精度に軸合わせしなくともある程度正確に読込できます  
  
## 変換基板仕様 
  * 3.3V電源(5Vpinと3.3Vpinが基板上で短絡)  
  * サイズ20mm x 15mm  
  * M3 x2 ピッチ15mm固定穴  
  * SPI端子出力ピンのみ  
  
  
## 使用可能エンコーダ
  * AS5048A [Digikey Link][1]  
  * AS4047D [Digikey Link][2]  
  * AS5047P [Digikey Link][3]  
  ※上記の全チップで動作確認している訳ではありません。  
  

## 使用例  
  * ネオジム磁石をタイヤや歯車等の軸中心の可動部に取り付けます  
  * エンコーダ基板のチップをネオジム磁石の中心と合わせて台車等の固定部に取り付けます　　
  * 下記は[ProjectinBall][4]での使用例です。ミラー部にネオジム磁石、基板部にエンコーダ変換基板が取り付けてあります  
  
 <img src="https://github.com/meerstern/Encoder_TSSOP14/blob/master/encoder-ex.png" width="360">
  
  
## プログラム例
  * AS5048A　SPI (Microchip C30)  
    ※MOSIは常にhighのため、MOSIを3.3Vへ接続し、3線通信でも可。
  
   int data0,data1,res;  
   SPICS1_OFF;  
	  putcSPI2(0xFF);  
	 	while(SPI2STATbits.SPITBF);  
	  while(!SPI2STATbits.SPIRBF);  
   data0=getcSPI2()&0xFF;  
   putcSPI2(0xFF);  
	 	while(SPI2STATbits.SPITBF);  
	  while(!SPI2STATbits.SPIRBF);  
   data1=getcSPI2()&0xFF;  
   res=((data0<<8)+data1)&0x3FFF;  
	  SPICS1_ON;  

  * AS5047D/P　SPI (Microchip C30)  
  
   int data0,data1,res;  
   SPICS1_OFF;  
	  putcSPI2(0x3F);  
	 	while(SPI2STATbits.SPITBF);  
	  while(!SPI2STATbits.SPIRBF);  
   data0=getcSPI2()&0xFF;  
   putcSPI2(0xFF);  
	 	while(SPI2STATbits.SPITBF);  
	  while(!SPI2STATbits.SPIRBF);  
   data1=getcSPI2()&0xFF;  
   res=((data0<<8)+data1)&0x3FFF;  
	  SPICS1_ON;    

License - MIT



[0]: http://ams.com/eng/Products/Magnetic-Position-Sensors "*0"
[1]: http://www.digikey.jp/product-detail/ja/ams/AS5048A-HTSP-500/AS5048A-HTSP-500CT-ND/3188617 "*1"
[2]: http://www.digikey.jp/product-detail/ja/ams/AS5047D-ATSM/AS5047D-ATSMCT-ND/4895563 "*2"
[3]: http://www.digikey.jp/product-detail/ja/ams/AS5047P-ATSM/AS5047P-ATSMCT-ND/5287312 "*3"
[4]: http://projectionball.jp/ "*4"
