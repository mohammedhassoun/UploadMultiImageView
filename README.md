
# UploadMultiImageView Multi-image upload component 
There is often a need to upload multiple images in projects. It is not a good idea to write it repeatedly every time. So based on **[recycleview]** to implement a multi-image upload component that only requires **[more than a dozen lines of code]** to have drag-and-drop sorting function.


If you are not currently using AndroidX, please upgrade as soon as possible!
# See the effect first

![Rendering](https://img-blog.csdnimg.cn/20200610222609657.gif#pic_center)

The library was copied and modified
You can view the parent library
[UploadMultiImageView source code portal](https://gitee.com/mtj_java/UploadMultiImageView)

# Instructions for use
## 1. Add: in the root directory build.gradle:

```java
allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}

```
## 2. Add in build.gradle under the module project:
```java
dependencies {
	        implementation 'com.github.mohammedhassoun:UploadMultiImageView:1.0.0'
	}
```

## 3. Add components to layout xml
```java
<com.binnishtech.multiimagelibrary.UploadMultiImageView
        android:id="@+id/uploadMultiImageView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#fff"
        app:addRes="@drawable/img_add"
        app:deleteRes="@drawable/img_delete"
        app:column_count="3"
        app:is_Drag="true"
        app:is_showAdd="true"
        app:item_spacing="2dp"
        app:max_count="9" />
```
# Version and custom attribute description
V1.1.0
--------------------------
New method:

```java
//Set the image scaling type (default CENTER_CROP)
setScaleType(ImageView.ScaleType.CENTER_CROP);
//Set delete click monitoring (if not set, the data will be removed directly by default) and handle the process yourself
setDeleteClickListener();
```

V1.0.0
--------------------------
1.0.0 Properties | Property Description
------------- | -------------
img_height | Image height (dp) defaults to equal to width
item_spacing| Spacing between pictures (dp) defaults to 2dp
column_count|Number of columns Default is 3 columns per row
max_count|Maximum number of pictures to display Default Integer.MAX_VALUE
is_showDelete|Whether to display the delete button, default true
is_showAdd|Whether to display the add button, default false
deleteRes|Set delete button
deleteWH|The width and height (dp) of the delete button defaults to 25dp
deleteResMargin|Margin (dp) of delete button, default 6dp
addRes|Set add button
is_Drag|Whether to enable drag sorting, default false

//Set the image scaling type (default CENTER_CROP)
setScaleType(ImageView.ScaleType.CENTER_CROP);
//Set delete click monitoring (if not set, the data will be removed directly by default) and handle the process yourself
setDeleteClickListener();

# Code implementation (written here in kotlin)## 1、Activity

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
// Loop to add several pieces of data
        val list: MutableList<ImageModel> = ArrayList()
        for (i in 0..7) {
            val model = ImageModel()
            model.path = R.mipmap.android
            list.add(model)
        }

        /**
         *The core implementation is here
         */
        uploadMultiImageView
                    .setImageInfoList(list.toList())
                     // All properties can be set in code
                     // Enable drag sorting
                    .setDrag(true)
                    //Set 3 columns per row
                    .setColumns(3)
                    // Show new button
                    .setShowAdd(true)
                    // Set image scaling type (default CENTERROP)
                    .setScaleType(ImageView.ScaleType.CENTER_CROP)
                    // item click callback
                    .setImageItemClickListener { position ->
                        //imageview click
                        Toast.makeText(baseContext, "Clicked on ${position}", Toast.LENGTH_SHORT).show()
                    }
                    // Set up deletion click monitoring (if not set, test to remove data by default), and handle the data deletion process yourself
                    .setDeleteClickListener { multiImageView, position ->
                        AlertDialog.Builder(this)
                            .setTitle("delete")
                            .setMessage("Are you sure you want to delete the picture?")
                            .setNegativeButton("Sure") { dialog, which ->
                                dialog.dismiss()
                                multiImageView.deleteItem(position)
                            }
                            .show()
                    }
                    // Image loading
                    .setImageViewLoader { context, path, imageView ->
                        // (You can choose the image loading frame here without any restrictions)
                        imageView.setImageResource(path as Int)
                    }
                    // Added button click callback
                    .setAddClickListener {
                        // Simulate adding a piece of data
                        addNewData()
                    }
                    .show()
    }

    /**
     * Add one or more pieces of data
     */
    private fun addNewData() {
        val tempList: MutableList<ImageModel> = ArrayList()
        val model = ImageModel()
        model.path = R.mipmap.android
        tempList.add(model)
        // Call new data
        uploadMultiImageView.addNewData(tempList.toList())
    }
}
```

## 2、Data entity class implementationImageInfo

```java
/**
 * accomplish ImageInfo
 */
public class ImageModel implements ImageInfo {

    private Object path;

    public Object getPath() {
        return path;
    }

    public void setPath(Object path) {
        this.path = path;
    }

    /**
     * @return  The map's address
     */
    @Override
    public Object getImagePath() {
        return path;
    }

    /**
     * @return Fixed return false
     */
    @Override
    public boolean isLastAddViewData() {
        return false;
    }
}
```
# Finished
How about it? Is it very simple? Does the core implementation only have a dozen lines of code?
