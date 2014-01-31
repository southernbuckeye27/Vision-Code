Vision-Code
===========

#What I have so far

import numpy as np
import cv2
import urllib2
import socket
import sys
import math
import os


img = cv2.imread('C:\Users\Joe 2.0\Pictures\Python Pics\Test2.jpg')
GlobalWidth = 640
GlobalHeight = 480
Channels = 3

#optionally resize by commenting out this line. It didn't have much impact, so no biggie.
resize = False
if resize: 
        img = cv2.resize(img, (GlobalWidth/2,GlobalHeight/2))
        GlobalWidth = GlobalWidth/2
        GlobalHeight = GlobalHeight/2
#pdb.set_trace()
#This portion rotates the image 90 degrees
rows,cols,channels = img.shape

M = cv2.getRotationMatrix2D((cols/2,rows/2), 90, 1)
img = cv2.warpAffine(img,M,(cols,rows))
#pdb.set_trace()
#show images on gui with cv2.imshow()
cv2.imshow('original_img', img)
cv2.waitKey(0)



#Grayscale
blue = cv2.split(img)[0]
cv2.imshow('original_img', blue)
cv2.waitKey(0)

#This portion may not work due to the fact the other code used a function found in the Processor class
#Photoshop, Opencv has differant numbers for HSV values, so they have to be converted before hand
def get_blue_hue (self, img):
        hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
        blue_vals = [120, 235, 235]
        upper = [140, 255, 255]
        lower = [100, 200, 200]
        only_blue = cv2.inRange(hsv_img, np.array(lower), np.array(upper))
        return only_blue

contours, hierarchy = cv2.findContours(blue, cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)


#pdb.set_trace()
blue_only = cv2.resize(blue, (GlobalWidth, GlobalHeight))
cv2.drawContours(blue_only, contours, -1, (156,156,156), 3)
cv2.imshow('all_contours', blue_only)
cv2.waitKey(0)

#Contours
#Contour-Moments
cnt = contours[0]
M = cv2.moments(cnt)
print M

showprint = True

hull = cv2.convexHull(cnt)


#Contour-Area/Perimeter
#area = cv2.contourArea(cnt)
#perimeter = cv2.arcLength(cnt,True)
area = 0
idx = -1
for i,cnt in enumerate(contours):
        if (800 < cv2.contourArea(cnt) < 20000):
                rect = cv2.convexHull(cnt)
                minRect = cv2.minAreaRect(rect)
                x1 = int(minRect[0][0])
                y1 = int(minRect[0][1])
                width = minRect [1][0]
                height = minRect[1][1]
                degree = minRect[2]
                if showPrint:

                        print 'x1', x1
                        print 'y1', y1
                        print 'width:', width
                        print 'height:', height
                        print 'degree:', degree
                #pdb.set_trace()
                if minRect[1][0]:
                        ratio = minRect[1][1] / minRect[1][0]
                else:
                        ratio = 0
                if showPrint: print 'ratio', ratio

                if ((2.9 < ratio < 3.3) or (0.25 < ratio < 0.37)):
                        if showPrint: print 'winning ratio:', ratio

                if (area <cv2.contourArea(cnt)):
                        idx = i
                """
                if(area < cnt.size):
                        idx = i
                """

img = cv2.rectangle(img, (384, 0), (510,128),(0,255,0),3)

#Range Finder
#minVal < -DBL_MAX
#maxVal < DBL_MAX
#pos != NULL
#quiet = true
#cv2.checkRange(a[, quiet[, 0[, 100]]])


#if (idx != -1):
        #if showPrint:		
                #cv2.drawContours(goals_img, contours, idx, (50, 255, 60), 3)
                #cv2.imshow('rects', goals_img)

        #rect = cv2.convexhull(contours[idx])
        #minRect = cv2.minAreaRect(rect)


#if __name__ == '__main__':
        #processor = Processor()


                
cv2.destroyWindow('original_img')
cv2.destroyAllWindows() 

