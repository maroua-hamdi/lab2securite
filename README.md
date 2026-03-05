LAB 2 : Rooting Android 

Étape 1 : Rooter l'AVD


Commandes exécutées
adb root
adb remount
adb shell id
adb shell getprop ro.boot.verifiedbootstate
adb shell getprop ro.boot.veritymode
adb shell getprop ro.boot.vbmeta.device_state
adb shell "su -c id"


