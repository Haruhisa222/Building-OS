
# ゼロから始める自作OS

## OSとは

### OSの三つの側面
1. **アプリに対するインタフェース**  
   アプリと周辺機器などの間を仲介する
2. **計算資源の分配**  
   CPUの処理をどれを優先させるか管理する
3. **人間に対するインタフェース**  
   各アプリに統一されたメニューを表示させる

### インタフェース
- **POSIX**  
  UNIXで使われる。Cの処理やファイルシステム
- **Windows API**  
  Microsoft独自のOSインタフェース

-> 本書では既存インタフェースに依拠しないインタフェースにする

---

## ブートローダ（第一章〜第三章）

### ブートローダとは
- 「OSをメインメモリに読み込み起動させるプログラム」のこと
- CPUは 原則としてRAM(メインメモリ)上の命令しか実行できない。
- 基本はOSがROM(ストレージ)にあるデータ,プログラムをRAMに渡している
- PC起動時は例外で、PCは電源投入直後OSの存在すら知らないので、ROMに、OSをメインメモリに読み込み起動させるプログラム(ブートローダ)を入れておく必要がある

### hello, world するバイナリコードを書いてみる
- バイナリコードにも、どのルールで書くか、がある

-> 本書では、x86-64 アーキテクチ（64ビットCPU向けの機械語のルールの一つ）で書いた

### ファームウェア
- PCの電源を入れた直後に最初に動くソフトウェアのこと
- 昔の名残で、今でもファームウェア全体をBIOSと呼ぶ場合もある

1. **BIOS**  
   1980年代PCのファームウェア
2. **EFI**  
   BIOSの設計を捨てて作り直した
3. **UEFI**  
   EFIを標準化し、現在広く採用されている

###  hello, world するバイナリコードをUSBに書き込み、EFIで起動
1. MACでUSBを初期化し起動時にEFIが呼ばれるようにした
    sudo diskutil partitionDisk disk4 GPT MS-DOS UEFIUSB 100% 

2. 起動するPCで、起動時にUSBが呼ばれるようにした
    - FujitsuノートPCのBIOS画面でBoot->UEFI Priorities->Boot Option #1をELECOM MF...(コード入りUSB)に変更
    - Security->Secure Boot->Disabledに変更
- 補足
    - レッツノートでもF2連打->BIOS画面で再起動 でいけた
    - GPTに聞いたら別の場所にコードを保存していて動かなかったが、https://qiita.com/ktamido/items/56b3427827894021ed73 を参考にしてやれば動いた⇩
            
            mkdir -p ~/mnt/esp

            sudo diskutil mount -mountPoint ~/mnt/esp /dev/disk4s1

            mkdir -p ~/mnt/esp/EFI/BOOT
            cp BOOTX64.EFI ~/mnt/esp/EFI/BOOT/BOOTX64.EFI

            diskutil unmountDisk /dev/disk4
- 起動順の整理

    1.PC起動-> 2.BIOS起動 ->3.バイナリコード/ブートローダ等のBIOSアプリケーションを実行
    

### 







