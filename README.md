# Image_Calibration-distortion

## **Introduction**
체스보드를 여러 방면에서 찍은 영상을 CV를 활용하여 Camera Calibration과 Distortion Correction 기능을 구현하고자 한다.

## **Developer**
* 임민규(Lim Mingyu)

## **Release**
체스보드를 카메라 통해 여러 각도에서 찍은 동영상을 활용한다.

## **Set Up & Prerequisites**
* Python >= 3.9
* Install python pip.
  * You may need to install first:<br>
    `python -m pip install opencv-python`<br>
    `pip install numpy`

## **Description**
**코드에 대한 내용은 주석을 참**

* Camera Calibration
  * 2D 코너 포인트를 찾았을 시 저장
    ```
    img_points = []  

    for img in images:
        gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)  
        complete, pts = cv.findChessboardCorners(gray, board_pattern)  
        if complete:  
            img_points.append(pts)
    assert len(img_points) > 0
    ```
    
  * 체스보드의 3D 오브젝트 포인트 준비
    ```
    bj_pts = [[c, r, 0] for r in range(board_pattern[1]) for c in range(board_pattern[0])]
    obj_points = [np.array(obj_pts, dtype=np.float32) * board_cellsize] * len(img_points)
    ```
* Distortion Correction
  * 렌즈 왜곡을 보정
    ```
    h, w = image.shape[:2]
    new_camera_matrix, roi = cv.getOptimalNewCameraMatrix(camera_matrix, dist_coeffs, (w, h), 1, (w, h))
    undistorted_image = cv.undistort(image, camera_matrix, dist_coeffs, None, new_camera_matrix)
    # Crop the image based on the region of interest
    x, y, w, h = roi
    undistorted_image = undistorted_image[y:y+h, x:x+w]
    return undistorted_image
    ```


      
