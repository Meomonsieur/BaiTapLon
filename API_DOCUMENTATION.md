# App Doc Truyen - Comprehensive API Documentation

## Overview

**App Doc Truyen** is an Android comic/manga reading application built in Java. The app allows users to browse, search, and read comics with chapter navigation functionality. It follows the MVC (Model-View-Controller) pattern with Activities handling UI interactions, adapters managing list displays, and data models representing comic information.

### Architecture

- **Activities**: Handle user interface and navigation
- **Adapters**: Manage list and grid view data binding
- **Models**: Represent data structures for comics and chapters
- **API Integration**: Network communication using Volley library
- **Image Loading**: Glide library for efficient image loading and caching

### Key Features

- Browse comic collection with grid layout
- Search functionality with real-time filtering
- Chapter listing for selected comics
- Page-by-page comic reading interface
- Network-based content loading
- Image caching and optimization

---

## Core Activities

### MainActivity

**Package**: `com.example.appdoctruyen`  
**Purpose**: Main entry point displaying the comic collection with search functionality

#### Public Methods

##### `protected void onCreate(Bundle savedInstanceState)`
Initializes the main activity, sets up UI components, and loads comic data.

**Parameters:**
- `savedInstanceState`: Bundle containing saved state information

**Usage Example:**
```java
// Automatically called by Android system
// Sets up:
// - Grid view for comics
// - Search functionality  
// - Refresh button
// - Network data loading
```

##### `public void GetJsonArray(Context context)`
Fetches comic data from remote API and populates the grid view.

**Parameters:**
- `context`: Application context for network operations

**API Endpoint:** `http://192.168.1.8:8080/get/get.php`

**Usage Example:**
```java
MainActivity mainActivity = new MainActivity();
mainActivity.GetJsonArray(this);
```

**Response Format:**
```json
[
  {
    "id": "1",
    "tenTruyen": "Comic Title",
    "tenChuong": "Chapter Name", 
    "linkAnh": "https://example.com/image.jpg"
  }
]
```

#### Private Methods

##### `private void init()`
Initializes data structures and adapter.

##### `private void anhXa()`
Maps UI components to class variables.

##### `private void setUp()`
Configures the grid view adapter.

##### `private void setClick()`
Sets up click listeners for search and item selection.

---

### ChapterActivity

**Package**: `com.example.appdoctruyen`  
**Purpose**: Displays chapter list for a selected comic

#### Public Methods

##### `protected void onCreate(Bundle savedInstanceState)`
Initializes chapter view with comic information and loads chapter data.

**Parameters:**
- `savedInstanceState`: Bundle containing saved state information

**Data Flow:**
1. Receives `ItemTruyen` object from MainActivity
2. Displays comic title and cover image
3. Loads chapter list from API
4. Handles chapter selection navigation

##### `public void GetChapJsonArray(Context context)`
Fetches chapter data for the selected comic.

**Parameters:**
- `context`: Application context for network operations

**API Endpoint:** `http://192.168.1.8:8080/get/getChap.php?id={comic_id}`

**Usage Example:**
```java
ChapterActivity chapterActivity = new ChapterActivity();
chapterActivity.GetChapJsonArray(this);
```

**Response Format:**
```json
[
  {
    "tenChap": "Chapter 1: Beginning",
    "ngayDang": "2024-01-15"
  }
]
```

---

### DocTruyenActivity

**Package**: `com.example.appdoctruyen`  
**Purpose**: Comic reading interface with page navigation

#### Public Methods

##### `protected void onCreate(Bundle savedInstanceState)`
Initializes the reading interface with comic pages.

##### `public void left(View view)`
Navigates to the previous page.

**Parameters:**
- `view`: The button view that triggered the action

**Usage Example:**
```xml
<Button
    android:onClick="left"
    android:text="Previous" />
```

##### `public void right(View view)`
Navigates to the next page.

**Parameters:**
- `view`: The button view that triggered the action

**Usage Example:**
```xml
<Button
    android:onClick="right"
    android:text="Next" />
```

#### Private Methods

##### `void DocTruyen(int i)`
Core page navigation logic.

**Parameters:**
- `i`: Direction (-1 for previous, 1 for next, 0 for current)

---

## Data Models

### ItemTruyen

**Package**: `com.example.appdoctruyen`  
**Purpose**: Represents a comic item with metadata
**Implements**: `Serializable`

#### Fields

| Field | Type | Description |
|-------|------|-------------|
| `tenTruyen` | String | Comic title |
| `tenChuong` | String | Latest chapter name |
| `linkAnh` | String | Cover image URL |
| `id` | String | Unique comic identifier |

#### Constructors

##### `ItemTruyen(JSONObject j) throws JSONException`
Creates ItemTruyen from JSON response.

**Parameters:**
- `j`: JSONObject containing comic data

**Usage Example:**
```java
JSONObject jsonObject = response.getJSONObject(i);
ItemTruyen comic = new ItemTruyen(jsonObject);
```

##### `ItemTruyen(String tenTruyen, String tenChuong, String linkAnh)`
Creates ItemTruyen with manual data.

**Parameters:**
- `tenTruyen`: Comic title
- `tenChuong`: Chapter name  
- `linkAnh`: Image URL

#### Public Methods

##### Getters
- `String getTenTruyen()`: Returns comic title
- `String getTenChuong()`: Returns chapter name
- `String getLinkAnh()`: Returns image URL
- `String getId()`: Returns comic ID

##### Setters
- `void setTenTruyen(String tenTruyen)`: Sets comic title
- `void setTenChuong(String tenChuong)`: Sets chapter name
- `void setLinkAnh(String linkAnh)`: Sets image URL
- `void setId(String id)`: Sets comic ID

---

### ChapTruyen

**Package**: `com.example.appdoctruyen`  
**Purpose**: Represents a comic chapter with metadata

#### Fields

| Field | Type | Description |
|-------|------|-------------|
| `tenChap` | String | Chapter title |
| `ngayDang` | String | Publication date |

#### Constructors

##### `ChapTruyen()`
Default constructor.

##### `ChapTruyen(String tenChap, String ngayDang)`
Creates chapter with manual data.

**Parameters:**
- `tenChap`: Chapter title
- `ngayDang`: Publication date

##### `ChapTruyen(JSONObject chap) throws JSONException`
Creates chapter from JSON response.

**Parameters:**
- `chap`: JSONObject containing chapter data

**Usage Example:**
```java
JSONObject jsonObject = response.getJSONObject(i);
ChapTruyen chapter = new ChapTruyen(jsonObject);
```

#### Public Methods

##### Getters
- `String getTenChap()`: Returns chapter title
- `String getNgayDang()`: Returns publication date

##### Setters
- `void setTenChap(String tenChap)`: Sets chapter title
- `void setNgayDang(String ngayDang)`: Sets publication date

---

## Adapter Classes

### TruyenTranhAdapter

**Package**: `com.example.appdoctruyen.adapter`  
**Extends**: `ArrayAdapter<ItemTruyen>`  
**Purpose**: Manages comic grid display and search functionality

#### Constructor

##### `TruyenTranhAdapter(Context context, int resource, List<ItemTruyen> objects)`

**Parameters:**
- `context`: Application context
- `resource`: Layout resource ID (typically 0)
- `objects`: List of ItemTruyen objects

**Usage Example:**
```java
ArrayList<ItemTruyen> comics = new ArrayList<>();
TruyenTranhAdapter adapter = new TruyenTranhAdapter(this, 0, comics);
gridView.setAdapter(adapter);
```

#### Public Methods

##### `void SortTruyen(String s)`
Filters comics based on search query and reorders the list.

**Parameters:**
- `s`: Search query string

**Algorithm:**
1. Converts search query to uppercase
2. Iterates through comic list
3. Moves matching items to front of list
4. Maintains original order for non-matching items

**Usage Example:**
```java
adapter.SortTruyen("naruto"); // Filters comics containing "naruto"
```

##### `View getView(int position, View convertView, ViewGroup parent)`
Creates and configures view for each grid item.

**Parameters:**
- `position`: Item position in list
- `convertView`: Recycled view (can be null)
- `parent`: Parent ViewGroup

**Layout**: `R.layout.item_truyen`

---

### ChapTruyenAdapter

**Package**: `com.example.appdoctruyen.adapter`  
**Extends**: `ArrayAdapter<ChapTruyen>`  
**Purpose**: Manages chapter list display

#### Constructor

##### `ChapTruyenAdapter(Context context, int resource, ArrayList<ChapTruyen> objects)`

**Parameters:**
- `context`: Application context
- `resource`: Layout resource ID (typically 0)
- `objects`: ArrayList of ChapTruyen objects

**Usage Example:**
```java
ArrayList<ChapTruyen> chapters = new ArrayList<>();
ChapTruyenAdapter adapter = new ChapTruyenAdapter(this, 0, chapters);
listView.setAdapter(adapter);
```

#### Public Methods

##### `View getView(int position, View convertView, ViewGroup parent)`
Creates and configures view for each chapter item.

**Parameters:**
- `position`: Item position in list
- `convertView`: Recycled view (can be null)
- `parent`: Parent ViewGroup

**Layout**: `R.layout.item_chap_truyen`

---

## API Integration

### Network Configuration

The app uses **Volley** library for network operations with the following configuration:

#### Base URLs
- Comics List: `http://192.168.1.8:8080/get/get.php`
- Chapters List: `http://192.168.1.8:8080/get/getChap.php?id={comic_id}`

#### Request Types
- **JsonArrayRequest**: Used for fetching lists of comics and chapters
- **GET Method**: All API calls use GET requests

#### Error Handling
```java
new Response.ErrorListener() {
    @Override
    public void onErrorResponse(VolleyError error) {
        // Silent error handling - no user notification
        // Consider adding proper error handling for production
    }
}
```

### Image Loading

Uses **Glide** library for efficient image loading:

```java
Glide.with(context)
     .load(imageUrl)
     .into(imageView);
```

**Features:**
- Automatic caching
- Memory management
- Placeholder support
- Error handling

---

## Usage Examples

### Basic Integration

#### 1. Setting up MainActivity
```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        // Initialize components
        init();
        anhXa();
        setClick();
        
        // Load data
        GetJsonArray(this);
    }
}
```

#### 2. Implementing Search
```java
EditText searchField = findViewById(R.id.editDS);
searchField.addTextChangedListener(new TextWatcher() {
    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
        String searchText = s.toString();
        adapter.SortTruyen(searchText);
    }
    // ... other methods
});
```

#### 3. Handling Item Selection
```java
gridView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
    @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
        ItemTruyen selectedComic = comicsList.get(position);
        
        // Pass data to next activity
        Bundle bundle = new Bundle();
        bundle.putSerializable("chap", selectedComic);
        
        Intent intent = new Intent(MainActivity.this, ChapterActivity.class);
        intent.putExtra("data", bundle);
        startActivity(intent);
    }
});
```

### Advanced Usage

#### Custom Adapter Implementation
```java
public class CustomTruyenAdapter extends TruyenTranhAdapter {
    public CustomTruyenAdapter(Context context, int resource, List<ItemTruyen> objects) {
        super(context, resource, objects);
    }
    
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View view = super.getView(position, convertView, parent);
        
        // Add custom modifications
        // Example: Add favorite button, rating display, etc.
        
        return view;
    }
}
```

#### Network Error Handling
```java
public void GetJsonArrayWithErrorHandling(Context context) {
    RequestQueue queue = Volley.newRequestQueue(context);
    String url = "http://192.168.1.8:8080/get/get.php";
    
    JsonArrayRequest request = new JsonArrayRequest(url,
        new Response.Listener<JSONArray>() {
            @Override
            public void onResponse(JSONArray response) {
                // Success handling
                processComicData(response);
            }
        }, 
        new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Proper error handling
                if (error.networkResponse != null) {
                    int statusCode = error.networkResponse.statusCode;
                    Log.e("API_ERROR", "Status Code: " + statusCode);
                }
                
                Toast.makeText(context, "Failed to load comics", Toast.LENGTH_SHORT).show();
            }
        }
    );
    
    queue.add(request);
}
```

---

## Setup and Installation Guide

### Prerequisites

- **Android Studio**: Latest version
- **Java Development Kit**: Version 8 or higher
- **Android SDK**: API level 24 or higher
- **Internet Connection**: Required for API access

### Dependencies

Add these dependencies to your `build.gradle.kts` file:

```kotlin
dependencies {
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.11.0")
    implementation("androidx.constraintlayout:constraintlayout:2.1.4")
    implementation("com.github.bumptech.glide:glide:4.16.0")
    implementation("com.android.volley:volley:1.2.1")
    
    // Testing dependencies
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.test.ext:junit:1.1.5")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.5.1")
}
```

### Permissions

Add these permissions to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

For HTTP traffic (development only):
```xml
<application
    android:usesCleartextTraffic="true"
    ... >
```

### Build Configuration

Configure your `build.gradle.kts`:

```kotlin
android {
    namespace = "com.example.appdoctruyen"
    compileSdk = 34

    defaultConfig {
        applicationId = "com.example.appdoctruyen"
        minSdk = 24
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"
    }

    buildFeatures {
        viewBinding = true
    }
    
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
}
```

### API Server Setup

The app requires a backend server providing these endpoints:

1. **Comics List Endpoint**
   - URL: `http://your-server:8080/get/get.php`
   - Method: GET
   - Response: JSON array of comic objects

2. **Chapters List Endpoint**
   - URL: `http://your-server:8080/get/getChap.php?id={comic_id}`
   - Method: GET
   - Response: JSON array of chapter objects

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd BaiTapLon-master
   ```

2. **Open in Android Studio**
   - File → Open → Select project directory

3. **Configure API URLs**
   - Update server URLs in `MainActivity.java` and `ChapterActivity.java`
   - Replace `192.168.1.8:8080` with your server address

4. **Build and Run**
   - Build → Make Project
   - Run → Run 'app'

### Troubleshooting

#### Common Issues

1. **Network Security Error**
   - Add `android:usesCleartextTraffic="true"` for HTTP endpoints
   - Use HTTPS in production

2. **Image Loading Issues**
   - Verify image URLs are accessible
   - Check Glide configuration

3. **JSON Parsing Errors**
   - Validate API response format
   - Add proper exception handling

#### Debug Tips

```java
// Enable Volley logging
RequestQueue queue = Volley.newRequestQueue(context);
queue.addRequestFinishedListener(new RequestQueue.RequestFinishedListener<Object>() {
    @Override
    public void onRequestFinished(Request<Object> request) {
        Log.d("VOLLEY", "Request finished: " + request.getUrl());
    }
});
```

---

## Best Practices

### Code Organization
- Follow package structure for better maintainability
- Use proper naming conventions
- Implement proper error handling

### Performance Optimization
- Use ViewHolder pattern in adapters
- Implement proper image caching
- Add pagination for large datasets

### Security Considerations
- Use HTTPS for production APIs
- Implement proper input validation
- Add authentication if required

### Testing
- Add unit tests for data models
- Implement UI testing for critical flows
- Test network error scenarios

---

## Contributing

When extending this application:

1. **Follow existing patterns**: Maintain consistency with current architecture
2. **Add proper documentation**: Update this documentation for new features
3. **Include examples**: Provide usage examples for new APIs
4. **Test thoroughly**: Ensure all functionality works across different devices
5. **Handle errors gracefully**: Implement proper error handling and user feedback

---

*Last Updated: 2024*
*Version: 1.0*