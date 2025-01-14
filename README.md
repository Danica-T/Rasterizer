# Rasterizer

This project was completed in 2022 for COS426: Computer Graphics @ Princeton University. The code implements a rasterizer used to render 3D objects and textures in an efficient manner.

## Description

The implemented filters and operations are as follows:

* Perspective Projection

I  went through each vertex's coordinates (3 since it is a triangle) and applied the projection matrix given. I divided by the "w" value to transform the 4D coordinates to 3D coordinates. If the projected vertex was not in the bounds of canonical view, then I returned undefined since you wouldn't see it. Otherwise, I scaled the projected vertex into screen coordinates and returned these values.

![PerspectiveProjection.png](https://github.com/Danica-T/Rasterizer/blob/main/results/PerspectiveProjection.png)

* Phong Reflection Model

Ambient color was implemented by adding the phongMaterial.ambient to the exisitng color. To find the specular color, I followed the formula given for the last assignment, Ray Tracing. I first found the reflected vector using the light vector and normal vector. Then I found the vector of the camera view by normalizing the view vector. The next step in the formula was to take the dot product of the negated view vector (since the camera view is flipped) and the reflection vector. We want to bound this value by 0 otherwise it would not be going towards the camera (viewer would not see). We take this value and put it to the power of the shininess value of the material. Finally, the specular value of the material is multiplied by this calculated value and added to the existing color. 

* Bounding Box

I found the bounding box values by comparing the respective projected vertex coordinate values and taking either the max or minimum of the set according to what was needed. I then bounded the max and min values by the screen size, since anything outside of those bounds are not seen. 

* Barycentric Coordinates

I followed the given formula for calculating Barycentric Coordinates. If the values found were less than 0, this means that the coordinate was outside the given triangle and should not be further calculated. 

* Flat Shader

I first found the normal of the surface by averaging the given vertex normals and found the face centroid by averaging the vertex coordinates. I then went through every pixel in the triangle by looping through the pixels within the bounding box of the triangle. 

For each pixel in the bounding box, I found the its Barycentric coordinate within the triangle. If the barycentric coordinate did not exist, that meant that the pixel was not in the triangle and we should not calculate it's color any further. 

I found the pixel's z coordinate by multiplying the projected vertices' z cooridnates by the barycentric cooridnates. If this z was closer than the buffered z, then I continued to render this pixel.

I interpolated the uv value using the barycentric cooridnates found and used this value to find the phong material of the pixel. I then found the color of the pixel by using the implemented function, phongReflectedModel. 

I then rendered the pixel with the newly found color. Finally, I updated the z buffer at the pixel with the found z coordinate found. 

![owFlat.png](https://github.com/Danica-T/Rasterizer/blob/main/results/cowFlat.png)

* Gouraud Shader

I first found each phong material for each uv value given. I then found each color that corresponded to each of those materials.  I then went through every pixel in the triangle by looping through the pixels within the bounding box of the triangle. 

For each pixel in the bounding box, I found the its Barycentric coordinate within the triangle. If the barycentric coordinate did not exist, that meant that the pixel was not in the triangle and we should not calculate it's color any further. 

I found the pixel's z coordinate by multiplying the projected vertices' z cooridnates by the barycentric cooridnates. If this z was closer than the buffered z, then I continued to render this pixel.

I interpolated the color values using the barycentric cooridnates found and used the sum of these colors to render the pixel. Finally, I updated the z buffer at the pixel with the found z coordinate found. 

![cowGouraud.png](https://github.com/Danica-T/Rasterizer/blob/main/results/cowGouraud.png)

* Phong Shader

I first went through every pixel in the triangle by looping through the pixels within the bounding box of the triangle. 

For each pixel in the bounding box, I found the its Barycentric coordinate within the triangle. If the barycentric coordinate did not exist, that meant that the pixel was not in the triangle and we should not calculate it's color any further. 

I found the pixel's z coordinate by multiplying the projected vertices' z cooridnates by the barycentric cooridnates. If this z was closer than the buffered z, then I continued to render this pixel.

I then interpolated the normals of each vertex. To find the uv, I interpolated the given uvs. If there existed a an xyzNromal for the material, I found the RGB value at u, v for that material. I used these values to update the face normal to equal the normal XYZ (which is the normalized values of 2*RGB - 1).

I then interpolated the vertexes by the barycentric coordinates and used this to find the face centroid value. Next, I found the color of the pixel by using the implemented function, phongReflectedModel. Using the newly found color, I rendered the pixel. Finally, I updated the z buffer at the pixel with the found z coordinate found. 

![cowPhong.png](https://github.com/Danica-T/Rasterizer/blob/main/results/cowPhong.png)

* Diffuse and Specular Mapping

For each shader, I checked if uv was no undefined, then interpolated the uv pixesl with the found barycentric coordinates. I used this value found to look up the phong material at the pixel.

![DiffuseSpecularMapping.png](https://github.com/Danica-T/Rasterizer/blob/main/results/DiffuseSpecularMapping.png)

* XYZ Normal Mapping

If there existed a an xyzNromal for the material, I found the RGB value at u, v for that material. I used these values to 
update the face normal to equal the normal XYZ (which is the normalized values of 2*RGB - 1).

![XYZNormalMapping.png](https://github.com/Danica-T/Rasterizer/blob/main/results/XYZNormalMapping.png)
