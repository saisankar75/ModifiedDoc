# Volley Library in Android

Volley is an HTTP library that makes networking very easy and fast, for Android apps. It was developed by Google and introduced during Google I/O 2013. It was developed because there is an absence in Android SDK, of a networking class capable of working without interfering with the user experience. Although Volley is a part of the Android Open Source Project(AOSP), Google announced in January 2017 that Volley will move to a standalone library. It manages the processing and caching of network requests and it saves developers valuable time from writing the same network call/cache code again and again.

Volley is not suitable for large download or streaming operations since Volley holds all responses in memory during parsing.

## Features of Volley:

- Request queuing and prioritization
- Effective request cache and memory management
- Extensibility and customization of the library to our needs
- Cancelling the requests

## Advantages of using Volley:
- All the task that need to be done with Networking in Android, can be done with the help of Volley.
- Automatic scheduling of network requests.
- Catching
- Multiple concurrent network connections.
- Cancelling request API.
- Request prioritization.
- Volley provides debugging and tracing tools.

## How to Import Volley and add Permissions:

Before getting started with Volley, one needs to import Volley and add permissions in the Android Project. The steps to do so are as follows:
Open build.gradle(Module: app) and add the following dependency:
```gradle
dependencies{ 
    //...
   implementation 'com.android.volley:volley:1.1.0'
}
```
In AndroidManifest.xml add the internet permission:

```xml 
<uses-permission
    android:name="android.permission.INTERNET />"
```
Classes in Volley Library:

### Volley has two main classes:

**Request Queue:** It is the interest one uses for dispatching requests to the network. One can make a request queue on demand if required, but typically it is created early on, at startup time, and keep it around and use it as a Singleton.


**Request:** All the necessary information for making web API call is stored in it. It is the base for creating network requests(GET, POST).

#### Codelab For Volley Library:

In this code lab we are going to create Covid19Tracker application By using the below api
[https://api.covid19api.com/](https://api.covid19api.com/)

- Lets create one new AndroidStudio project with the name of Covid19Tracker
- First we will display the countries list by using the below link
    [https://api.covid19api.com/countries](https://api.covid19api.com/countries)
    
**activity_main.xml:**
```xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerview"
        android:layout_width="409dp"
        android:layout_height="729dp"
        android:layout_marginStart="8dp"
        android:layout_marginLeft="8dp"
        android:layout_marginTop="50dp"
        android:layout_marginEnd="8dp"
        android:layout_marginRight="8dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

**MainActivity.Java**
```java
package com.example.covid19tracker;

import androidx.appcompat.app.AppCompatActivity;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import android.os.Bundle;
import android.util.Log;
import android.widget.Toast;

import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.JsonArrayRequest;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

import java.util.Arrays;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private RequestQueue mRequestQueue;
    String countriesURl = "https://api.covid19api.com/countries";
    private Gson gson;
    RecyclerView rv;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        rv = findViewById(R.id.recyclerview);
        mRequestQueue = Volley.newRequestQueue(this);
        gson = new GsonBuilder().create();
        StringRequest request = new StringRequest(Request.Method.GET, countriesURl, new Response.Listener<String>() {
            @Override
            public void onResponse(String response) {
                Toast.makeText(MainActivity.this,
                        "" + response, Toast.LENGTH_SHORT).show();
                List<Country> countryList = Arrays.asList(gson.fromJson(response,Country[].class));
                Log.i("SIZE",""+countryList.size());
                rv.setLayoutManager(new LinearLayoutManager(MainActivity.this));
                rv.setAdapter(new CountryAdapter(countryList,MainActivity.this));


            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {

            }
        });

        mRequestQueue.add(request);
    }


}

```
**Country.Java**
```java
package com.example.covid19tracker;

import com.google.gson.annotations.SerializedName;

public class Country {

    @SerializedName("Country")
    private String countryname;

    @SerializedName("ISO2")
    private String countrycode;

    public Country(String countryname, String countrycode) {
        this.countryname = countryname;
        this.countrycode = countrycode;
    }

    public String getCountryname() {
        return countryname;
    }

    public void setCountryname(String countryname) {
        this.countryname = countryname;
    }

    public String getCountrycode() {
        return countrycode;
    }

    public void setCountrycode(String countrycode) {
        this.countrycode = countrycode;
    }
}

```
**country_item.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <TextView
        android:id="@+id/country_name"
        android:layout_width="0dp"
        android:layout_height="50dp"
        android:layout_marginStart="16dp"
        android:layout_marginLeft="16dp"
        android:layout_marginTop="5dp"
        android:layout_marginEnd="16dp"
        android:layout_marginRight="16dp"
        android:background="#4B0857"
        android:gravity="center"
        android:text="TextView"
        android:textColor="#FFF"
        android:textSize="20sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
**CountryAdapter.java**
```java
package com.example.covid19tracker;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

import java.util.List;

public class CountryAdapter extends RecyclerView.Adapter<CountryAdapter.ViewInfo> {
    List<Country> myList;
    Context ct;
    public CountryAdapter(List<Country> countryList, MainActivity mainActivity) {
        ct = mainActivity;
        myList = countryList;
    }

    @NonNull
    @Override
    public CountryAdapter.ViewInfo onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View v = LayoutInflater.from(ct).inflate(R.layout.country_item,parent,false);
        return new ViewInfo(v);
    }

    @Override
    public void onBindViewHolder(@NonNull CountryAdapter.ViewInfo holder, int position) {
            holder.c_name.setText(myList.get(position).getCountryname());
    }

    @Override
    public int getItemCount() {
        return myList.size();
    }

    public class ViewInfo extends RecyclerView.ViewHolder {
        TextView c_name;
        public ViewInfo(@NonNull View itemView) {
            super(itemView);
            c_name = itemView.findViewById(R.id.country_name);
        }
    }
}

```
**OutPut**
![](https://raw.githubusercontent.com/mastan511/MastanImages/master/CountriesList.png)

After displaying the countries List now we should have to display the each country covid status in the CountryDetailActivity for that we should have to send the country code to the CountryDetailActivity.java file . 

In the CountryAdapter.java file write this code

```java
public class ViewInfo extends RecyclerView.ViewHolder {
        TextView c_name;
        public ViewInfo(@NonNull View itemView) {
            super(itemView);
            c_name = itemView.findViewById(R.id.country_name);
            itemView.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    Intent i = new Intent(ct,CountryDetailActivity.class);
                    i.putExtra("countrycode",myList.get(getAdapterPosition()).getCountrycode());
                    ct.startActivity(i);
                }
            });
        }
    }
    
```

**activity_country_detail.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".CountryDetailActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recycler"
        android:layout_width="409dp"
        android:layout_height="729dp"
        android:layout_marginStart="1dp"
        android:layout_marginLeft="1dp"
        android:layout_marginTop="32dp"
        android:layout_marginEnd="1dp"
        android:layout_marginRight="1dp"
        android:layout_marginBottom="1dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>

```
**CountryDetailActivity.java**

```java
package com.example.covid19tracker;

import androidx.appcompat.app.AppCompatActivity;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import android.os.Bundle;
import android.util.Log;
import android.widget.Toast;

import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class CountryDetailActivity extends AppCompatActivity {
    String countryCovidUrl = "https://api.covid19api.com/dayone/country/";
    private RequestQueue mRequestQueue;
    private Gson gson;
    RecyclerView rv;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_country_detail);
        String code = getIntent().getStringExtra("countrycode");
        String mainUrl = countryCovidUrl+code;
        Log.i("URL",mainUrl);
        rv = findViewById(R.id.recycler);
        mRequestQueue = Volley.newRequestQueue(this);
        gson = new GsonBuilder().create();

        StringRequest request = new StringRequest(Request.Method.GET, mainUrl, new Response.Listener<String>() {
            @Override
            public void onResponse(String response) {
                Toast.makeText(CountryDetailActivity.this,
                        "" + response, Toast.LENGTH_SHORT).show();
                List<CountryCovidCases> list = Arrays.asList(gson.fromJson(response,CountryCovidCases[].class));
                Collections.reverse(list);
                Log.i("SIZE",""+list.size());
                rv.setLayoutManager(new LinearLayoutManager(CountryDetailActivity.this));
                rv.setAdapter(new CasesAdapter(list,CountryDetailActivity.this));


            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {

            }
        });

        mRequestQueue.add(request);



    }
}

```


**CountryCovidCases.java**

```java

package com.example.covid19tracker;

import com.google.gson.annotations.SerializedName;

public class CountryCovidCases {
    @SerializedName("Active")
    private String activecases;

    @SerializedName("Recovered")
    private String recoveredcases;

    @SerializedName("Deaths")
    private String deaths;

    @SerializedName("Date")
    private String date;

    public CountryCovidCases(String activecases, String recoveredcases, String deaths, String date) {
        this.activecases = activecases;
        this.recoveredcases = recoveredcases;
        this.deaths = deaths;
        this.date = date;
    }

    public String getActivecases() {
        return activecases;
    }

    public void setActivecases(String activecases) {
        this.activecases = activecases;
    }

    public String getRecoveredcases() {
        return recoveredcases;
    }

    public void setRecoveredcases(String recoveredcases) {
        this.recoveredcases = recoveredcases;
    }

    public String getDeaths() {
        return deaths;
    }

    public void setDeaths(String deaths) {
        this.deaths = deaths;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }
}


```
**item.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="10dp"
    android:layout_margin="2dp"
    android:background="#D0D87D">

    <TextView
        android:id="@+id/date"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:gravity="center"
        android:text="TextView"
        android:textSize="20sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginLeft="16dp"
        android:layout_marginTop="16dp"
        android:text="Active"
        android:textSize="20sp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/date" />

    <TextView
        android:id="@+id/textView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="92dp"
        android:layout_marginLeft="92dp"
        android:layout_marginTop="16dp"
        android:text="Recovered"
        android:textSize="20sp"
        app:layout_constraintStart_toEndOf="@+id/textView2"
        app:layout_constraintTop_toBottomOf="@+id/date" />

    <TextView
        android:id="@+id/textView5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="50dp"
        android:layout_marginLeft="50dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="16dp"
        android:layout_marginRight="16dp"
        android:text="Deaths"
        android:textSize="20sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toEndOf="@+id/textView4"
        app:layout_constraintTop_toBottomOf="@+id/date" />

    <TextView
        android:id="@+id/active"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginLeft="16dp"
        android:layout_marginTop="16dp"
        android:layout_marginBottom="2dp"
        android:text="TextView"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView2" />

    <TextView
        android:id="@+id/recover"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="114dp"
        android:layout_marginLeft="114dp"
        android:layout_marginTop="16dp"
        android:layout_marginBottom="2dp"
        android:text="TextView"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toEndOf="@+id/active"
        app:layout_constraintTop_toBottomOf="@+id/textView4" />

    <TextView
        android:id="@+id/deaths"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="86dp"
        android:layout_marginLeft="86dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="16dp"
        android:layout_marginRight="16dp"
        android:layout_marginBottom="2dp"
        android:text="TextView"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@+id/recover"
        app:layout_constraintTop_toBottomOf="@+id/textView5" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

**CasesAdapter.Java**

```java
package com.example.covid19tracker;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

import java.util.List;

public class CasesAdapter extends RecyclerView.Adapter<CasesAdapter.ViewInfo> {
    Context ct;
    List<CountryCovidCases> myList;
    public CasesAdapter(List<CountryCovidCases> list, CountryDetailActivity countryDetailActivity) {
        myList = list;
        ct = countryDetailActivity;

    }

    @NonNull
    @Override
    public CasesAdapter.ViewInfo onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View v = LayoutInflater.from(ct).inflate(R.layout.item,parent,false);
        return new ViewInfo(v);
    }

    @Override
    public void onBindViewHolder(@NonNull CasesAdapter.ViewInfo holder, int position) {
        holder.date.setText(myList.get(position).getDate());
        holder.active.setText(myList.get(position).getActivecases());
        holder.recover.setText(myList.get(position).getRecoveredcases());
        holder.deaths.setText(myList.get(position).getDeaths());

    }

    @Override
    public int getItemCount() {
        return myList.size();
    }

    public class ViewInfo extends RecyclerView.ViewHolder {
        TextView date,active,recover,deaths;
        public ViewInfo(@NonNull View itemView) {
            super(itemView);
            date = itemView.findViewById(R.id.date);
            active = itemView.findViewById(R.id.active);
            recover = itemView.findViewById(R.id.recover);
            deaths = itemView.findViewById(R.id.deaths);
        }
    }
}


```

**OutPut**

![](https://raw.githubusercontent.com/mastan511/MastanImages/master/caseslist.png)




















