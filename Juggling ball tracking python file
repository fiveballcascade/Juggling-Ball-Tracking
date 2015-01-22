import numpy as np
import cv2
import math
colors = [(255,0,0), (125,125,125), (0,255,0), (0,0,255), (125,125,0)]
new = 0
track = []
radius = 18
thickness = 5
cap = cv2.VideoCapture('/home/sm/Desktop/VIRB0012.MP4')
for i in range(930): ret, img = cap.read()
while True:
    ret, img = cap.read()
    img = img[0:480, 100:700]
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    ret, thresh = cv2.threshold(gray, 233, 255, 0)
    contours, h, g = cv2.findContours(thresh, cv2.RETR_TREE,
                                      cv2.CHAIN_APPROX_SIMPLE)
    locations = []
    for i in h:
        area = cv2.contourArea(i)
        if area > 300:
            location = cv2.minEnclosingCircle(i)
            locations.append(location[0])
    if len(locations) < len(track):
        print 'problem'
        min_distances = []
        for i in track:
            x,y = i[1][0], i[1][1]
            distance = []
            for c in locations:
                x1, y1 = c[0], c[1]
                distance.append(math.sqrt((x-x1)**2 + (y-y1)**2))
            min_distances.append(min(distance))
        index = min_distances.index(max(min_distances))
        locations.append(track[index][1])
    if new == 0:
        for i in range(len(locations)):
            track.append((colors[i], locations[i]))
        new = 1
        num_balls = len(locations)
    else:
        new_track = []
        for i in locations:
        x,y = i[0], i[1]
        distance = []
        for c in track:
            x1, y1 = c[1][0], c[1][1]
            distance.append(math.sqrt((x-x1)**2+(y-y1)**2))
        index = distance.index(min(distance))
        new_track.append((track[index][0], (x,y)))
    track = new_track                

    for i in track:
        center = int(i[1][0]),int(i[1][1])
        cv2.circle(img, center, radius, i[0], thickness)
    cv2.imshow('Detected Balls', img)
    cv2.waitKey(100)
cv2.destroyAllWindows()
cv2.release(cap)
