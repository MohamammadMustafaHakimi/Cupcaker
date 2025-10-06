1. Things I learned
-> Android Sharesheet: a composable, which is used to share data; in other words, whenever you share applications you you encounter it
-> In **Kotlin (for Android development)**, an **Intent** is a messaging object used to request an action from another component — such as starting a new activity, opening a webpage, sending an email, etc.

   Let’s go through it **step by step**, intuitively and with examples 👇

   ---

   ## 🧩 What is an Intent?

   An **Intent** expresses an *intention* to do something.
   For example:

   * “Open the Settings app.”
   * “Start a new screen (Activity).”
   * “Send data to another Activity.”

   In code, it’s just an object that carries **action** + **data** + **optional extras**.

   ---

   ## ⚙️ Types of Intents

   ### 1. **Explicit Intents**

   Used to start a **specific Activity** or **Service** *inside your own app*.

   You explicitly name the target class.

   #### Example – Start another activity:

   ```kotlin
   // Inside MainActivity.kt
   val intent = Intent(this, SecondActivity::class.java)
   intent.putExtra("username", "Mustafa")
   startActivity(intent)
   ```

   #### Receiving data in SecondActivity:

   ```kotlin
   // Inside SecondActivity.kt
   val username = intent.getStringExtra("username")
   textView.text = "Hello, $username!"
   ```

   ---

   ### 2. **Implicit Intents**

   Used when you want the system to decide *which app* can handle your request.
   You **don’t specify a class name**, just an action (and sometimes data).

   #### Example – Open a webpage:

   ```kotlin
   val intent = Intent(Intent.ACTION_VIEW)
   intent.data = Uri.parse("https://www.google.com")
   startActivity(intent)
   ```

   #### Example – Send an email:

   ```kotlin
   val intent = Intent(Intent.ACTION_SENDTO).apply {
       data = Uri.parse("mailto:")
       putExtra(Intent.EXTRA_EMAIL, arrayOf("someone@example.com"))
       putExtra(Intent.EXTRA_SUBJECT, "Hello from Kotlin!")
   }
   startActivity(intent)
   ```

   #### Example – Share text:

   ```kotlin
   val intent = Intent(Intent.ACTION_SEND).apply {
       type = "text/plain"
       putExtra(Intent.EXTRA_TEXT, "Check out my new app!")
   }
   startActivity(Intent.createChooser(intent, "Share via"))
   ```

   ---

   ## 📦 Passing Data Between Activities

   You can attach small pieces of data (called **extras**) to your Intent.

   | Type                              | Function         |
   | --------------------------------- | ---------------- |
   | `putExtra("key", value)`          | Add data         |
   | `getStringExtra("key")`           | Retrieve String  |
   | `getIntExtra("key", default)`     | Retrieve Int     |
   | `getBooleanExtra("key", default)` | Retrieve Boolean |

   Example:

   ```kotlin
   // Sending
   val intent = Intent(this, DetailActivity::class.java)
   intent.putExtra("age", 25)
   startActivity(intent)

   // Receiving
   val age = intent.getIntExtra("age", 0)
   ```

   ---

   ## 🔁 Start Activity for Result (deprecated way and new way)

   ### Old (deprecated):

   ```kotlin
   startActivityForResult(intent, REQUEST_CODE)
   ```

   ### New (recommended with Activity Result API):

   ```kotlin
   val launcher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
       if (result.resultCode == Activity.RESULT_OK) {
           val data = result.data?.getStringExtra("result")
           textView.text = data
       }
   }

   // Launch
   val intent = Intent(this, SecondActivity::class.java)
   launcher.launch(intent)
   ```

   And in `SecondActivity`:

   ```kotlin
   val resultIntent = Intent()
   resultIntent.putExtra("result", "Hello back!")
   setResult(Activity.RESULT_OK, resultIntent)
   finish()
   ```

   ---

   ## 🧠 Summary

   | Use Case                         | Intent Type | Example                                                |
   | -------------------------------- | ----------- | ------------------------------------------------------ |
   | Start another screen in your app | Explicit    | `Intent(this, SecondActivity::class.java)`             |
   | Open a URL                       | Implicit    | `Intent(Intent.ACTION_VIEW, Uri.parse("https://..."))` |
   | Share text                       | Implicit    | `Intent(Intent.ACTION_SEND)`                           |
   | Send email                       | Implicit    | `Intent(Intent.ACTION_SENDTO)`                         |
   | Get result back                  | Explicit    | Activity Result API                                    |

   ---

   Would you like me to show a **simple two-activity app example** (MainActivity → SecondActivity with data passing and result return)? It’s a very practical way to solidify your understanding.


-> a slash plus asterisk will still become a multi-line comment

/*
* so /* will make the lines after the comment section a comment
*
*/