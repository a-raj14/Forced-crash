# Forced-crash
Forced crash to make IL2CPP Build for Android.....

Method 1: (Array Out of bound Exception)

    static int func2(int[] x)
    {
		int y = x[11];
		return y;
    }
    static void _func1()
    {
        Thread.Sleep(4000);
        int[] x = new int[10];
        int y = x[1];
		y = func2(x);
        //Debug.Log(y.ToString());
    }

    public void crash()
	{
		Thread _t1 = new Thread(_func1);
        _t1.Start();
		throw new Exception("Test excption");
	}
  
 Method 2: (Divide by zero exception)//This may cause error sometimes if you are converting code from one lang to another .
 
 static void _func1()
	{
		Debug.LogError("Inside _func1");
		Debug.LogErrorAndSendToRemote("Inside Thread");
		Thread.Sleep(4000);
		int[] x = null;
		int y = x[1];
		Debug.Log(y.ToString());

	}

public void crash()
	{
		int i = 0;
		int k = 0;
		i = i / k;

		Thread _t1 = new Thread(_func1);
		_t1.Start();
		throw new Exception("Test excption");
	}
Method 3: Forced app crash (Ref for this is from stack overflow: https://stackoverflow.com/questions/17511070/android-force-crash-with-uncaught-exception-in-thread)

public void CrashOnAndroid()
		{
			// https://stackoverflow.com/questions/17511070/android-force-crash-with-uncaught-exception-in-thread
			var message = new AndroidJavaObject("java.lang.String", "This is a test crash, ignore.");
			var exception = new AndroidJavaObject("java.lang.Exception", message);

			var looperClass = new AndroidJavaClass("android.os.Looper");
			var mainLooper = looperClass.CallStatic<AndroidJavaObject>("getMainLooper");
			var mainThread = mainLooper.Call<AndroidJavaObject>("getThread");
			var exceptionHandler = mainThread.Call<AndroidJavaObject>("getUncaughtExceptionHandler");
			exceptionHandler.Call("uncaughtException", mainThread, exception);
		}
