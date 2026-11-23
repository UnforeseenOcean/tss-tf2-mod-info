# tss-tf2-mod-info
Compiled info for modifying t 0 ss f R 0n t 2 prior to 9.13 (Dead-end patch)

## Opening the device and entering programming mode
1. Take the bottom hatch off
2. Remove the rubber feet
3. Remove 5 visible screws
4. Insert a flat-head screwdriver or spudger into each rectangular hole where the tongues of the hatch cover go, and pull it outward while pulling the back cover up
5. Remove the 4 visible screws where the display panel is situated
6. Remove the grey cosmetic cover from the back of the device
7. Undo 2 screws
8. Insert guitar picks around the seam near the speaker hole and release the back cover carefully
9. Look at the connector with yellow tape and rainbow-colored wires on it
10. Look below the third and fourth pins of the connector
11. You will see `1V8` and `BOOT`; note the position
12. Insert a Micro USB cable (recommended: get a new one with data lines; do not use USB hubs or front USB ports) into the matching connector on the board
13. While shorting it with a pair of tweezers, **BEING SURE NOT TO TOUCH 12V PAD or nearby connector pins**, plug the USB-C cable in (If you short 1V8 or BOOT to DP or GND or 12V, you're screwed. Recommended action: Solder two thin wires to those pads and wire it up to a button newly installed in the empty spot below the hatch)
14. If successful, you will hear a sound on your computer that tells you new hardware is connected
15. If not, check the USB cable and tweezer position (again, if you do it wrong enough or if your hand slips, you might inject lethal voltage into the CPU power rail and kill it)

Note: On older devices 7.1 or earlier, press and hold the power button while plugging the USB-C cable in to enter programming mode (the device may appear to be not booting, but if it shows up as `Qualcomm HS-USB QDLoader 9008` or `QUSB_BULK_CID...` then you're good to go)

If you see an exclamation mark next to the device name, it means you need to use Zadig to install the WinUSB driver; go and do it

## Using programming mode
You will use tools made by B.Kerler available on GitHub

Note: Compatibility differs, and the required elf fire hose file differs from version to version
- If your version number ends with 9.13, you're screwed
- If your version number ends with 9.5 or 9.1, you may be in luck, but you will need to look around a lot more
- If your version number is less than 7.1, then you're good to go; one of Xiaomi's tablets with the same CPU will have this elf file
- You must source the loader yourself; it is not here and will not be linked here
1. `python edl.py printgpt --loader=firehose.elf` to test if you are in luck or not (If you see the partition table being printed, you are in luck and can continue)
2. `python edl.py r swraw swraw.img --loader=firehose.elf` to dump SWRAW data
3. Using a hex editor, add `SYSDEBUGMODE=true` at offset 0x300 (null-terminated; do not hit enter)
4. Now add `APPDEBUGMODE=true` at offset 0x400 (null-terminated)
5. Check again to see if you made any typos or wrong byte edits
6. Save as a new file
7. `python edl.py w swraw (new image name).img` to write the config (This is also the last chance to undo anything you messed up; if you made a mistake, redo it immediately)
8. `python edl.py reset` and it will reboot immediately
9. If you have done it correctly, you will see a transparent red bar at the top of the screen with the version number and SW number upon booting into the OS (If not: You're screwed)
10. Now you are ready to use ADB
11. **DO NOT CONNECT TO THE INTERNET**
12. Press the "Connect to network" button on the app
13. **DO NOT CONNECT TO THE INTERNET**
14. Swipe from the top of the screen to open the menu
15. **DO NOT CONNECT TO THE INTERNET**
16. Swipe again for the full menu
17. Tap on the cogwheel
18. Go to About Device
19. Spam the build number until the Developer menu opens
20. Enable USB Debugging
21. Check if you can see ADB devices
22. If not, allow USB debugging from the prompt that appears (use `adblink` for easier management)
23. BACK UP YOUR SYSTEM!!! If you screw it up, you will have a 0% chance of recovery without backups
24. `adb reboot fastboot` if you are lucky
25. `fastboot devices`
26. `fastboot -w`
27. Procure a LineageOS GSI ROM image (But not 20260105; it is known to boot loop)
28. `fastboot flash system system.img`
29. It will now flash the image. DO NOT MOVE OR TURN OFF THE DEVICE!
30. If it says `Not enough space` then `fastboot delete-logical-partition product_a` and `fastboot delete-logical-partition product_b`
31. If it STILL says so, `fastboot delete-logical-partition system_ext_a` and `fastboot delete-logical-partition system_ext_b`
32. `fastboot -w`
33. `fastboot reboot`
34. If all went well, you will boot into LineageOS instead of whatever was on the device

## Not lucky?
Wait for a new version of fire hose

If you typed `adb reboot fastboot` and it rudely said "Fastboot is disabled", well, you're out of luck and have to wait for the new version of fire hose again

## Got lucky, but not ready to commit yet?
Just fetch the full package list and uninstall anything that has the word tee-oh-ess-ess in it with `adb shell pm uninstall --user 0 (package name)` then install F-Droid to use apps

Note that you cannot use GApps on this version of Android; this means no Google Play Store and Play Services, and no apps that rely on this feature will work at all (i.e., Spotify, YT Music, YouTube, etc.)

## Screwed up and connected to the Internet?
First of all, how dare you, I told you not to do it, you are an idiot and I hate you

Second of all, if you see a red bar at the top of the screen anyway, you're in luck. Refer to the section above to use it in a slightly compromised fashion

# Korean 한국어
9.13 버전(더 이상 업데이트가 지원되지 않는 '데드엔드' 패치) 이전의 t 0 ss f R 0n t 2 기기 개조를 위한 정보 모음

## 기기 분해 및 프로그래밍 모드 진입
1. 하단 덮개를 분리합니다.
2. 고무 받침대를 제거합니다.
3. 눈에 보이는 나사 5개를 제거합니다.
4. 하단 덮개의 고정 돌기가 들어가는 직사각형 구멍에 일자 드라이버나 스퍼저(분해용 도구)를 삽입한 뒤, 이를 바깥쪽으로 밀면서 뒷면 덮개를 들어 올려 분리합니다.
5. 디스플레이 패널 쪽에 있는 나사 4개를 제거합니다.
6. 기기 뒷면의 회색 커버를 제거합니다.
7. 나사 2개를 제거합니다.
8. 스피커 구멍 근처의 틈새에 기타 피크를 끼워 넣어 뒷면 덮개를 조심스럽게 분리합니다.
9. 노란색 테이프가 붙어 있고 무지개색 전선이 연결된 커넥터를 찾습니다.
10. 해당 커넥터의 세 번째 핀과 네 번째 핀 아래쪽을 확인합니다.
11. `1V8` 및 `BOOT`라고 표시된 부분을 찾고 그 위치를 기억해 둡니다.
12. 보드상의 해당 커넥터에 마이크로 USB 케이블을 연결합니다(데이터 전송이 가능한 새 케이블을 사용하는 것을 권장하며, USB 허브나 PC 전면 USB 포트는 사용하지 마십시오).
13. 핀셋을 사용하여 해당 지점을 쇼트(단락)시킵니다. 이때 **12V 패드나 주변 커넥터 핀에 닿지 않도록 각별히 주의**하면서 USB-C 케이블을 연결하십시오(1V8 또는 BOOT 핀이 DP, GND, 12V 핀과 쇼트되면 기기가 완전히 고장 날 수 있습니다. 권장 방법: 해당 패드에 얇은 전선 두 가닥을 납땜으로 연결한 뒤, 하단 덮개 아래 빈 공간에 버튼을 새로 설치하여 이 전선들을 연결하는 것입니다).
14. 성공적으로 진행되었다면, 컴퓨터에서 새로운 하드웨어가 연결되었음을 알리는 소리가 들릴 것입니다.
15. 그렇지 않다면 USB 케이블과 핀셋의 위치를 ​​다시 확인하십시오(주의: 작업 중 실수가 발생하거나 손이 미끄러질 경우, CPU 전원 라인에 치명적인 전압이 유입되어 CPU가 완전히 고장 날 수 있습니다).

참고: 7.1 이하 버전의 구형 기기에서는 전원 버튼을 길게 누른 상태에서 USB-C 케이블을 연결하면 프로그래밍 모드로 진입합니다(기기가 부팅되지 않는 것처럼 보일 수 있지만, `Qualcomm HS-USB QDLoader 9008` 또는 `QUSB_BULK_CID...`와 같은 이름으로 인식된다면 정상적으로 진입된 것입니다).

기기 이름 옆에 느낌표가 표시된다면, Zadig을 사용하여 WinUSB 드라이버를 설치해야 한다는 의미입니다. 해당 작업을 진행해 주십시오.

## 프로그래밍 모드 사용하기
GitHub에서 제공되는 B.Kerler 제작 도구를 사용하게 됩니다.

참고: 호환성은 기기마다 다르며, 필요한 'elf firehose' 파일도 버전에 따라 다릅니다.
- 버전 번호가 9.13으로 끝난다면, 사실상 해결이 불가능합니다.
- 버전 번호가 9.5나 9.1로 끝난다면 가능성이 있을 수 있으나, 추가적인 정보를 훨씬 더 많이 찾아봐야 합니다.
- 버전 번호가 7.1 미만이라면 문제없습니다. 동일한 CPU를 사용하는 샤오미(Xiaomi) 태블릿 중 하나에서 해당 elf 파일을 찾을 수 있을 것입니다.
- 로더(loader) 파일은 직접 구하셔야 합니다. 본 문서에는 포함되어 있지 않으며, 관련 링크도 제공하지 않습니다.

1. `python edl.py printgpt --loader=firehose.elf` 명령어를 실행하여 성공 여부를 확인합니다(파티션 테이블이 출력된다면 성공한 것이며, 다음 단계를 진행할 수 있습니다).
2. `python edl.py r swraw swraw.img --loader=firehose.elf` 명령어를 사용하여 SWRAW 데이터를 덤프(추출)합니다.
3. 헥스 에디터(hex editor)를 사용하여 오프셋 0x300 위치에 `SYSDEBUGMODE=true`를 추가합니다(널 문자로 종료되어야 하며, 엔터 키를 누르지 마십시오).
4. 마찬가지로 오프셋 0x400 위치에 `APPDEBUGMODE=true`를 추가합니다(널 문자로 종료).
5. 오타가 있거나 잘못된 바이트를 수정하지 않았는지 다시 한번 확인합니다.
6. 새로운 파일로 저장합니다.
7. `python edl.py w swraw (새 이미지 파일명).img` 명령어를 사용하여 설정값을 기록합니다(이 단계가 잘못된 부분을 되돌릴 수 있는 마지막 기회입니다. 실수를 발견했다면 즉시 다시 작업하십시오).
8. `python edl.py reset` 명령어를 실행하면 기기가 즉시 재부팅됩니다.
9. 제대로 작업했다면, OS로 부팅된 후 화면 상단에 버전 번호와 SW 번호가 표시된 반투명한 빨간색 막대가 나타납니다(나타나지 않는다면... 상황이 매우 좋지 않은 것입니다).
10. 이제 ADB를 사용할 준비가 되었습니다.
11. **인터넷에 연결하지 마십시오**
12. 앱에서 "네트워크에 연결(Connect to network)" 버튼을 누르십시오
13. **인터넷에 연결하지 마십시오**
14. 화면 상단에서 아래로 스와이프하여 메뉴를 여십시오
15. **인터넷에 연결하지 마십시오**
16. 한 번 더 스와이프하여 전체 메뉴를 여십시오
17. 톱니바퀴 아이콘을 누르십시오
18. '기기 정보(About Device)'로 이동하십시오
19. '빌드 번호(build number)' 항목을 여러 번 연속으로 눌러 '개발자 옵션(Developer menu)'을 활성화하십시오
20. 'USB 디버깅(USB Debugging)'을 활성화하십시오
21. ADB 기기가 인식되는지 확인하십시오
22. 인식되지 않는다면, 화면에 나타나는 메시지에서 USB 디버깅을 허용하십시오(더 쉽게 관리하려면 `adblink`를 사용하십시오)
23. **시스템을 백업하십시오!!!** 백업 없이 잘못 진행할 경우, 복구할 가능성은 0%입니다
24. 운이 좋다면 `adb reboot fastboot` 명령어를 실행하십시오
25. `fastboot devices`
26. `fastboot -w`
27. LineageOS GSI ROM 이미지를 준비합니다(단, 20260105 버전은 피하세요. 부팅이 무한 반복되는(boot loop) 문제가 알려져 있습니다).
28. `fastboot flash system system.img` 명령어를 실행합니다.
29. 이제 이미지가 플래싱됩니다. 기기를 움직이거나 전원을 끄지 마세요!
30. `Not enough space`(공간 부족) 오류가 발생하면, `fastboot delete-logical-partition product_a` 및 `fastboot delete-logical-partition product_b` 명령어를 실행하세요.
31. 그래도 같은 오류가 발생한다면, `fastboot delete-logical-partition system_ext_a` 및 `fastboot delete-logical-partition system_ext_b` 명령어를 실행하세요.
32. `fastboot -w` 명령어를 실행합니다.
33. `fastboot reboot` 명령어를 실행합니다.
34. 모든 과정이 정상적으로 완료되었다면, 기존에 설치되어 있던 운영체제 대신 LineageOS로 부팅될 것입니다.

## 운이 없으신가요?
새로운 버전의 '파이어 호스(fire hose)'가 나올 때까지 기다리세요.

`adb reboot fastboot` 명령어를 입력했는데 "Fastboot is disabled(패스트부트가 비활성화되었습니다)"라는 냉정한 메시지가 떴다면, 안타깝게도 운이 따르지 않은 것입니다. 새로운 버전의 파이어 호스가 나올 때까지 다시 기다려야 합니다.

## 운은 좋았지만, 아직 완전히 변경하기는 망설여지시나요?
전체 패키지 목록을 불러온 뒤, 이름에 'tee-oh-ess-ess'가 포함된 항목들을 `adb shell pm uninstall --user 0 (패키지 이름)` 명령어로 삭제하세요. 그 후 F-Droid를 설치하여 앱을 사용하시면 됩니다.

참고로, 이 안드로이드 버전에서는 GApps(구글 앱)를 사용할 수 없습니다. 즉, 구글 플레이 스토어나 플레이 서비스가 제공되지 않으며, 이를 필요로 하는 앱(Spotify, YT Music, YouTube 등)은 전혀 작동하지 않습니다.

## 실수로 인터넷에 연결해 버리셨나요?
우선, 어떻게 그러실 수 있죠? 분명히 하지 말라고 말씀드렸잖아요! 정말 바보 같으시네요, 정말 싫어요!

둘째로, 만약 화면 상단에 빨간색 막대가 나타난다면 그나마 다행입니다. 앞서 설명한 섹션을 참고하여, 기능이 다소 제한된 상태로 기기를 사용해 보세요.
