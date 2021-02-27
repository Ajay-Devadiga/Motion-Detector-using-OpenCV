# Motion-Detector-using-OpenCV
Python script that uses OpenCV library to detect motion on live security camera and play buzzer sounds to alert its owner

Libraries required : **OpenCV** (cmd to install : "pip install opencv-python")
                     **winsound** (ONLY work on windows platforms.)
                     
                     
  ###############Import Libraires############################
import cv2
import winsound


#Initialize camera
mycam = cv2.VideoCapture(0)                 #('0' is you have default camera or add '1' or '2' depending on respective number of external camera)#


while cam.isOpened():
  ret, frame1 = cam.read()
  ret, frame2 = cam.read()

diff = cv2.absdiff(frame1, frame2)          #(to measure difference between two adjacent frames)#
gray = cv2.cvtColor(diff, cv2.COLOR_BGR2GRAY)
blur = cv2.GaussianBlur(gray, (5,5), 0)
_, thresh = cv2.threshold(blur, 20, 255, cv2.THRESH_BINARY)
dialtedFrame = cv2.dilate(thresh, None, iterations=3)
contours, _ = cv2.findContours(dialtedFrame, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
for c in contours:
  if cv2.contourArea(c) < 3000:
      continue
  x, y, w, h = cv2.boundingRect(c)
  cv2.rectangle(frame1, (x,y), (x+w, y+h), (0, 0, 255), 4)
  winsound.PlaySound('residential-burglar-siren-1657_ajay.wav', winsound.SND_ASYNC) #(place this .wav file in root directory)
  winsound.Beep(5000, 200)

if cv2.waitKey(10) == ord('q'):
  break
# Comment below lines if frames are rotated
frame1 = cv2.rotate(frame1, cv2.ROTATE_180)
cv2.imshow("Security Camera", frame1)
        
