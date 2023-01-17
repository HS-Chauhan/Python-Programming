#from lib2to3.pytree import _Results
import enum
from operator import truediv
from sre_constants import SUCCESS
import cv2
import mediapipe as mp
import math
import numpy as np
from ctypes import cast, POINTER
from comtypes import CLSCTX_ALL
from pycaw.pycaw import AudioUtilities, IAudioEndpointVolume


devices = AudioUtilities.GetSpeakers()
interface = devices.Activate(
    IAudioEndpointVolume._iid_, CLSCTX_ALL, None)
volume = cast(interface, POINTER(IAudioEndpointVolume))

# print("Volume Level :" , volume.GetMasterVolumeLevel())
# print("Volume Range :" , volume.GetVolumeRange())
# volume.SetMasterVolumeLevel(-10.0, None)


mpDraw = mp.solutions.drawing_utils
#mp_drawing_styles = mp.solutions.drawing_styles
mpHands = mp.solutions.hands
hands = mpHands.Hands()

#THIS IS THE CODE TO CAPTURE THE IMAGE

cap = cv2.VideoCapture(0)

while True:
    success, img = cap.read()
    imgRGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    results = hands.process(imgRGB)
    #print(results.multi_hand_landmarks) #THIS LINE IS TO NOTICE THE POSITION OF THE HAND IN THE IMAGE


    if results.multi_hand_landmarks:
        for handLms in results.multi_hand_landmarks:# HERE WE USED FOR LOOP SO THAT ALL HANDS THAT ARE CAPTURED CAN BE LANDMARKED
            lmList = []
            for id, lm in enumerate(handLms.landmark):#THIS AND THE NEXT LINES ARE CREATED TO PREVENT SCROLLING WHEN NO HANDS ARE SHOWN
                #print(id, lm)
                h , w , c = img.shape
                #IN THE ABOVE LINE ARE HEIGHT WIDTH AND CHANNEL(H,W,C)
                cx, cy = int(lm.x*w) , int(lm.y*h)
                #print(id, cx, cy)
                lmList.append([id, cx, cy])


            #print(lmList)#THIS AND THE ABOVE LINE WERE USED TO GIVE THE HAND POSITIONS IN THE FORM OF AN ARRAY    

            # mpDraw.draw_landmarks(img, handLms, mpHands.HAND_CONNECTIONS)  
            if lmList:
                x1, y1 = lmList[4][1] , lmList[4][2]
                x2, y2 = lmList[8][1] , lmList[8][2]
            cv2.circle(img, (x1,y1), 10 , (3,6,245) , cv2.FILLED)    
            cv2.circle(img, (x2,y2), 10 , (3,6,245) , cv2.FILLED) 
            cv2.line(img , (x1,y1) , (x2,y2), (72,46,132), 3) 
            length = math.hypot(x2-x1 , y2-y1)
            print(length)  

            volRange = volume.GetVolumeRange()
            minVol = volRange[0]
            maxVol = volRange[1]
            vol = np.interp(length , [50,300], [minVol, maxVol])

            volume.SetMasterVolumeLevel(vol, None)



    cv2.imshow("YourImage", img)
    cv2.waitKey(1)

    # length = 50 ==> 300
    # Volume = 0 ==> 100
    # Volume Range = -65.25 ==> 0.0

