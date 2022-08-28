# AndroidJenkins
Jenkins file for android
![Screenshot 2022-08-28 192742](https://user-images.githubusercontent.com/17147992/187078656-22318aef-d0ba-4d39-97bd-3f2fa3438143.png)

Jenkins Official Site : https://www.jenkins.io/doc/book/

Java Installation.  $ java -version 

Space Requirements. $ df -h

OS : $ cat /etc/os-release. 

War Installations : https://www.jenkins.io/doc/book/installing/war-file/

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null

wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
  

sudo apt-get update 
sudo apt-get install fontconfig openjdk-11-jre
sudo apt-get install jenkins


Get Admin Passoword
nano /var/lib/jenkins/secrets/initialAdminPassword

==> 307e6ead4bd146009683e8d657053b05  


admin user
user : jenkins
pass : 
name : mohit

Process on port : lsof -i:9000
Kill all process:kill -9 $(lsof -t -i:9000)



CHECK VIRTUALIZATION IF YOU WANT TO RUN EMULATOR FOR UI/INSTRUMENTATION TESTS
=================>

Warning: File /var/lib/jenkins/.android/repositories.cfg could not be loaded.
[android] Using Android SDK: /var/lib/jenkins/tools/android-sdk
$ /var/lib/jenkins/tools/android-sdk/platform-tools/adb start-server
* daemon not running; starting now at tcp:5850
* daemon started successfully
$ /var/lib/jenkins/tools/android-sdk/platform-tools/adb start-server
[android] Starting Android emulator
$ /var/lib/jenkins/tools/android-sdk/emulator/emulator -skin 480x800 -ports 5770,5771 -report-console tcp:5837,max=60 -prop persist.sys.language=en -prop persist.sys.country=US -avd hudson_en-US_240_WVGA_android-23_google_apis-x86_mySuffix -no-window
emulator: ERROR: Failed to retrieve DNS servers list from the system
emulator: WARNING: Cannot find system DNS servers! Name resolution will be disabled.
emulator: ERROR: x86 emulation currently requires hardware acceleration!
Please ensure KVM is properly installed and usable.
CPU acceleration status: KVM requires a CPU that supports vmx or svm
[android] Emulator did not appear to start; giving up
[android] Stopping Android emulator


$ egrep -c ‘(svm|vmx)’ /proc/cpuinfo

egrep -c svm /proc/cpuinfo
egrep -c vmx /proc/cpuinfo

lscpu


Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                2
On-line CPU(s) list:   0,1
Thread(s) per core:    2
Core(s) per socket:    1
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 63
Model name:            Intel(R) Xeon(R) CPU @ 2.30GHz
Stepping:              0
CPU MHz:               2300.000
BogoMIPS:              4600.00
Virtualization:        VT-x
Hypervisor vendor:     KVM
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              46080K
NUMA node0 CPU(s):     0,1
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc eagerfpu pni pclmulqdq vmx ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm tpr_shadow flexpriority ept fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid xsaveopt arat arch_capabilities
[mohit_singh@jenkins ~]$ cat /sys/module/kvm_intel/parameters/nested
N

[mohit_singh@jenkins ~]$ cat /etc/os-release

NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"


===================>


https://serverfault.com/questions/22748/quick-way-to-check-if-your-processor-has-virtual-machine-extensions

Paravirtualization is virtualization in which the guest operating system (the one being virtualized) is aware that it is a guest and accordingly has drivers that, instead of issuing hardware commands, simply issue commands directly to the host operating system. This also includes memory and thread management as well, which usually require unavailable privileged instructions in the processor.

Full Virtualization is virtualization in which the guest operating system is unaware that it is in a virtualized environment, and therefore hardware is virtualized by the host operating system so that the guest can issue commands to what it thinks is actual hardware, but really are just simulated hardware devices created by the host.

Hardware Assisted Virtualization is a type of Full Virtualization where the microprocessor architecture has special instructions to aid the virtualization of hardware. These instructions might allow a virtual context to be setup so that the guest can execute privileged instructions directly on the processor without affecting the host. Such a feature set is often called a Hypervisor. If said instructions do not exist, Full Virtualization is still possible, however it must be done via software techniques such as Dynamic Recompilation where the host recompiles on the fly privileged instructions in the guest to be able to run in a non-privileged way on the host.

There is also a combination of Para Virtualization and Full Virtualization called Hybrid Virtualization where parts of the guest operating system use paravirtualization for certain hardware drivers, and the host uses full virtualization for other features. This often produces superior performance on the guest without the need for the guest to be completely paravirtualized. An example of this: The guest uses full virtualization for privileged instructions in the kernel but paravirtualization for IO requests using a special driver in the guest. This way the guest operating system does not need to be fully paravirtualized, since this is sometimes not available, but can still enjoy some paravirtualized features by implementing special drivers for the guest.

Check if nested virtualization is  on in host server
cat /sys/module/kvm_intel/parameters/nested


CREATE TAR
tar -czvf android_data.tar android_data


INSTALL ADB 
Go root by $sudo su 
apt install adb
chown this folder to jenkins


CREATE AVD
./avdmanager create avd --force --name testAVD --abi google_apis/x86 --package 'system-images;android-28;google_apis;x86'

./avdmanager create avd -f -n hudson_en-US_480_1080x1920_android-26_x86 -k system-images;android-26;default;x86


SDK MANAGER
./sdkmanager --install 'system-images;android-29;google_apis;x86'

ACCEPT LICENSES
CHOWN to jenkins
The SDK directory is not writable
chown -R jenkins $ANDROID_HOME


SWITCH TO JENKINS USER
sudo -i
su - jenkins -s /bin/bash


ADD Jenkins User to your Github Repository (Might have to share rsa with Server Admin team)


APP DISTRIBUTION
====================>

SETUP FIREBASE PROJECT

Download 
curl -sL https://firebase.tools | bash


AAB Distribution

release {
           firebaseAppDistribution {
               appId="yourAppId"
               artifactType="AAB"
               testers="your@exampleemail.com, cerseimartell.772371@email.com"
           }
       }

-PappDistribution-property-name=property-value




