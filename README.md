# retrofit Lobray Add in build.gradle

dependencies {
    compile 'com.squareup.retrofit2:retrofit:2.0.2'
    }

# Internet Access permission write in AndroidManifest

   uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"
    uses-permission android:name="android.permission.INTERNET" 
  

# Application Class

public class Appdemo extends Application 
{
    private static Appdemo apprkc;
    
    @Override
    public void onCreate() {
        super.onCreate();
        apprkc = this;
    }


     public static synchronized AppCloudLaundry getInstance() {
        return apprkc;
    }

  
    public DataAPI getAdapter() {
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(Comman_url.url)
                .build();
        DataAPI dataAPI = retrofit.create(DataAPI.class);
        return dataAPI;
    }

}

# Data Interface

public interface DataAPI 
{
    @GET("login.php")
    Call<ResponseBody> loginget(@Query("email") String email,
                              @Query("password") String password);

    @FormUrlEncoded
    @POST("login1.php")
    Call<ResponseBody> loginpost(@Field("email") String email,
                                @Field("password") String password);
}


# Lonin Activity

Request_loader Loader=new Request_loader(LoginActivity.this);

 private void login()
 {
        Loader.showpDialog();
        DataAPI apiCall = AppCloudLaundry.getInstance().getAdapter();
        Call<ResponseBody> responseBodyCall;

        responseBodyCall = apiCall.login(edt_email.getText().toString().trim(),edt_password.getText().toString().trim());

        responseBodyCall.enqueue(new Callback<ResponseBody>() {
            @Override
            public void onResponse(Call<ResponseBody> call, Response<ResponseBody> response) {
                try {

                    String responceResult = response.body().string();

                     Log.e("Responce", responceResult + "");
                    if (responceResult != null) {
                        try {
                            JSONObject jsonObjectTmp = new JSONObject(responceResult);
                            if(jsonObjectTmp.getString("status").equals("1"))
                            {
                               
                            }
                            else{
                                Toast.makeText(LoginActivity.this, "Oops! Server error, please try again.", Toast.LENGTH_SHORT).show();
                            }
                        } catch (JSONException e) {
                            //e.printStackTrace();
                            Toast.makeText(LoginActivity.this, "Oops! Server error, please try again.", Toast.LENGTH_SHORT).show();
                        }

                        // Log.e("Responce success ", responceResult);
                    }

                } catch (IOException e) {
                    // Log.e("ResponceError", e.getMessage());
                    Toast.makeText(LoginActivity.this, "Oops! Server error, please try again.", Toast.LENGTH_SHORT).show();
                }
                Loader.hidepDialog();
            }



public class Request_loader 
{
	private Context context;
	private Dialog please_wait_dialog;

	public Request_loader(Context context) {
		super();
		this.context = context;
		please_wait_dialog = new Dialog(context);
		please_wait_dialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
		please_wait_dialog.setContentView(R.layout.progress);
		please_wait_dialog.setCancelable(false);
		final Window window = please_wait_dialog.getWindow();
	    window.setLayout(WindowManager.LayoutParams.MATCH_PARENT, WindowManager.LayoutParams.MATCH_PARENT);
	    //window.clearFlags(WindowManager.LayoutParams.FLAG_DIM_BEHIND);
	    window.setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));
		

	}


	public void showpDialog() {
		if (!please_wait_dialog.isShowing()){
			please_wait_dialog.show();}
	}

	public void hidepDialog() {
		if (please_wait_dialog.isShowing()){
			please_wait_dialog.dismiss();}
	}
	
	 
}


