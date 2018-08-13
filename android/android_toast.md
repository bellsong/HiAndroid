如何长时间展示一个Toast？

```
Display  Toast  for longer time
In this example we will show the toast for 20Seconds (20000 milliseconds)

public class ToastActivity extends Activity
{
    AlertDialog dialog;
    
     static CountDownTimer timer =null;
     Toast toast;
     @Override
        public void onCreate(Bundle savedInstanceState) 
        {
                super.onCreate(savedInstanceState);
                
                // creating toast and setting properties
                toast = new Toast(this);
                TextView textView=new TextView(this);
                textView.setTextColor(Color.BLUE);
                textView.setBackgroundColor(Color.TRANSPARENT);
                textView.setTextSize(20);
                textView.setText("This Toast will Display for 20 Seconds in Center of The Screen");
                toast.setGravity(Gravity.CENTER_VERTICAL, 0, 0);

                toast.setView(textView);
                
               //    Toast Display tTime Settings
                
                // Create the CountDownTimer object and implement the 2 methods
                // show the toast in onTick() method  and cancel the toast in onFinish() method
                // it will show the toast for 20 seconds (20000 milliseconds 1st argument) with interval of 1 second(2nd argument)
 
                timer =new CountDownTimer(20000, 1000)
                {
                    public void onTick(long millisUntilFinished)
                    {
                        toast.show();
                    }
                    public void onFinish()
                    {
                        toast.cancel();
                    }

                }.start();
                
          }
}
 
We can also cancel our toast before specified tine of 20 Seconds  on a particular condition with following code

// Cancelling toast on a particular condition

                if(your conditinal expression)
                {
                    timer.cancel();
                }

```