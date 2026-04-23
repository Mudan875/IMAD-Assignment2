# IMAD-Assignment2

Screen 1
This is the first screen of the programme it shows a discription of what the project is about 

<img width="402" height="700" alt="Screenshot 2026-04-23 143936" src="https://github.com/user-attachments/assets/67db3242-6b20-4244-9989-804e0dd07273" />







Screen 2 

This screen shows the user a statement and they have to choose whether if it is a myth or if its true and after they choose either of the button there will be a text view showing them whether if they got it right or wrong.

<img width="373" height="656" alt="Screenshot 2026-04-23 143947" src="https://github.com/user-attachments/assets/0876c0b1-8794-43e7-9942-512d8cf8764e" />
















Screen 3

This screen shows the total score of the amount of questions they got right

<img width="378" height="664" alt="Screenshot 2026-04-23 144000" src="https://github.com/user-attachments/assets/a79c0ebd-b0d2-4aba-9345-f9e53c036534" />



























Code for the first screen 

package za.ac.iie.assignment2

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
            //Declaration
        val btnStart = findViewById<Button>(R.id.btnStart)
// Code for the button btnStart to go to the next screen
        btnStart.setOnClickListener {
            val intent = Intent(this, FlashCards::class.java)

            startActivity(intent)
        }
    }
}


Code for the second screen 

package za.ac.iie.assignment2

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class FlashCards : AppCompatActivity() {
    val questions = arrayOf( "Spicy food cures a cold",
    "Shaving makes your hair grow thicker",
    "Carrots dont improve night vision"
    )
    val answers =arrayOf(true , false , false)

    val explanations = arrayOf(
        "Water helps brain function ",
        "Does not alter the hair follicles",
        "Spicy food cannot cure a cold it can help with the cold temporarily"
    )

    override fun onCreate(savedInstanceState: Bundle?) {

        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_flash_cards)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
        // Declaration
        var currentIndex = 0
        var score = 0
        val tvStatement = findViewById<TextView>(R.id.tvStatement)
        val btnTrue = findViewById<Button>(R.id.btnTrue)
        val btnFalse = findViewById<Button>(R.id.btnFalse)
        val tvFeedback = findViewById<TextView>(R.id.tvFeedback)
        val btnNext = findViewById<Button>(R.id.btnNext)

        tvStatement.text = questions[currentIndex]
        // Displays whether if the users answer is correct of wrong
        fun checkAnswer(userAnswer : Boolean){
            if(userAnswer ==answers[currentIndex]){
                score++
                tvFeedback.text ="Correct"
            }else{
                tvFeedback.text= "Wrong"
            }
        }
        // checks if answer is true

        btnTrue.setOnClickListener {
            checkAnswer(true)
        }

        btnFalse.setOnClickListener {
            checkAnswer(false)
        }

        //Goes to the next screen after answering all the questions

        btnNext.setOnClickListener {
            currentIndex++

            if (currentIndex < questions.size) {
                tvStatement.text = questions[currentIndex]
                tvFeedback.text = ""
            }else{
                val intent = Intent(this , Score::class.java)
                intent.putExtra("score",score)
                intent.putExtra("total", questions.size)

                startActivity(intent)

            }
        }



    }
}

Code for the third screen

package za.ac.iie.assignment2

import android.annotation.SuppressLint
import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class Score : AppCompatActivity() {
    @SuppressLint("MissingInflatedId")
    override fun onCreate(savedInstanceState: Bundle?) {

        val questions = arrayOf( "Drinking water improves concentration",
            "Cracking knuckles causes arthritis",
            "Carrots dont improve night vision"
        )
        val answers =arrayOf(true , false , false)

        val explanations = arrayOf(
            "Water helps brain function ",
            "No scientific Proof",
            "Carrots dont improve night vision")
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_score)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
        // Declaration

        val score = intent.getIntExtra("score ", 0)
        val total = intent.getIntExtra("total", 0)

        val tvScore= findViewById<TextView>(R.id.tvScore)
        val tvFeed= findViewById<TextView>(R.id.tvFeed)
        val btnReview= findViewById<Button>(R.id.btnReview)
        val tvOutput = findViewById<TextView>(R.id.tvOutput)
        // Calculation for the score
        tvScore.text= "Score : $score / $total"

        if (score >= total /2) {
            tvFeed.text= "Great job"

        }else{
            tvFeed.text = "Keep practising!"
        }
        // Displays the review for the user
        btnReview.setOnClickListener {
            var output = ""

            for (i in questions.indices) {

                output += "Q : ${questions[i]}\n"
                output += "Answer : ${if (answers[i]) "Hack " else "Myth"}\n"
                output += "Explaination: ${explanations[i]}\n\n"
            }

        }


    }
}











