PROJECT TITLE: Programming 3B - Part 1


PURPOSE OF THE PROJECT: The purpose of the project was to write a C# software where the user had to choose between replacing books, identifying areas, and finding call numbers. When the user chooses to replace books, 10 random call numbers will be displayed, and the user can then reorder it.


DATE: 21 September 2023


AUTHORS: Nicole Willemse


USER INSTRUCTIONS:
1.	The user will be met with a splash screen, which will load until the welcome page appears.
2.	The user should then click on “Replace Books”, followed by “Display Books”.
3.	Here, the application will display 10 generated call numbers, where the user will be instructed to reorder them again in the box next to the generated call numbers.
4.	The user should then click on the “submit” button to check their sorting. Or, click in the “back” button to go back to the main page.
5.	The user can keep track of the score on top, on the right side of the application.



HOW TO COMPILE THIS PROJECT: 
1. Open Visual Studio.
2. On the start window, select Create a new project.
3. On the Create a new project window, select the Windows Forms App (.NETFramework) template for C#.
4. In the Configure your new project window, type or enter NewDewey in the Project name box. Then, select Create.

5. Now we create the first form called the splash screen:
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace NewDewey.Pages
{
    public partial class SplashScreen : Form
    {
        public SplashScreen()
        {
            InitializeComponent();
        }

        private void SplashScreen_Load(object sender, EventArgs e)
        {
            timer1.Start();
        }
        int startpoint = 0;
        private void timer1_Tick(object sender, EventArgs e)
        {
            startpoint += 1;
            SplashProgressIndicator1.Start();
            if (startpoint > 50)
            {
                Startup starting = new Startup();
                SplashProgressIndicator1.Stop();
                timer1.Stop();
                this.Hide();
                starting.Show();
            }
        }
    }
}


6. Next, we will create our second form called startup: 
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Runtime.CompilerServices;
using System.Security.Cryptography.X509Certificates;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace NewDewey.Pages
{
    public partial class Startup : Form
    {
        public Startup()
        {
            InitializeComponent();
            customizeDesign();
        }

        private void StartingPage_Load(object sender, EventArgs e)
        {
            //Disable the task from being selected
            DisableTaskArea();
            DisableTaskFindNumber();

        }

        private void label2_Click(object sender, EventArgs e)
        {

        }

        private void customizeDesign()
        {
            panel_repSubmenu.Visible = false;
            panel_fndSubmenu.Visible = false;
            panel_areaSubmenu.Visible = false;
        }

        private void hideSubmenu()
        {
            if (panel_repSubmenu.Visible == true)
                panel_repSubmenu.Visible = false;
            if (panel_fndSubmenu.Visible == true)
                panel_fndSubmenu.Visible = false;
            if (panel_areaSubmenu.Visible == true)
                panel_areaSubmenu.Visible = false;
        }

        private void showSubmenu(Panel submenu)
        {
            if (submenu.Visible == false)
            {
                hideSubmenu();
                submenu.Visible = true;
            }
            else
                submenu.Visible = false;
        }

        private void DisableTaskArea()
        {
            //Disable the task
            button_IdArea.Enabled = false;
        }

        private void DisableTaskFindNumber()
        {
            //Disable the task
            button_fndNumbers.Enabled = false;
        }

            private void button_repBooks_Click(object sender, EventArgs e)
        {
            showSubmenu(panel_repSubmenu);
        }

        private void button_displayBook_Click(object sender, EventArgs e)
        {
            ReplacingBooks Form = new ReplacingBooks();
            Form.Show();

         }

        private void button_IdArea_Click(object sender, EventArgs e)
        {
            showSubmenu(panel_areaSubmenu);
        }

        private void button_exploreArea_Click(object sender, EventArgs e)
        {
            //...
            //...Future code will come here
            //...
            hideSubmenu();
        }

        private void button_fndNumbers_Click(object sender, EventArgs e)
        {
            showSubmenu(panel_fndSubmenu);
        }

        private void button_disNumbers_Click(object sender, EventArgs e)
        {
            //...
            //...Future code will come here
            //...
            hideSubmenu();
        }

        //Exit the application
        private void button_exit_Click(object sender, EventArgs e)
        {
            this.Close();
        }
    }

}

7. Lastly we will create our last form called ReplacingBooks: 
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace NewDewey.Pages
{
    public partial class ReplacingBooks : Form
    {
        private List<string>callNumbers = new List<string>();
        private int score = 0;

        public ReplacingBooks()
        {
            InitializeComponent();
        }

        private void replacingBooks_Load(object sender, EventArgs e)
        {
            Random random = new Random();
            for (int i = 0; i < 10; i++)
            {
                string callNumber = $"{random.Next(1000):000}.{random.Next(100):00} {GetRandomAuthorSurname()}";
                callNumbers.Add(callNumber);
            }
            DisplayRandomCallNumbers();
        }
        
       private void DisplayRandomCallNumbers()
        {
            listBox_listNumbers.Items.Clear();
            foreach (string callNumber in callNumbers)
            {
                listBox_listNumbers.Items.Add(callNumber);
            }
        }

        private string GetRandomAuthorSurname()
        {
            Random random = new Random();
            const string letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
            return new string(Enumerable.Repeat(letters, 3)
                .Select(s => s[random.Next(s.Length)]).ToArray());
        }



        private bool CheckSorting(string[] userSortedNumbers)
        {
            // Check if the user's sorting matches the correct order.
            if (userSortedNumbers.Length != callNumbers.Count)
            {
                return false;
            }

            for (int i = 0; i < callNumbers.Count; i++)
            {
                if (string.Compare(callNumbers[i], userSortedNumbers[i], StringComparison.OrdinalIgnoreCase) != 0)
                {
                    return false;
                }
            }

            return true;
        }

        private void BubbleSort(List<string> list)
        {
            // Using bubble sort to sort the call number list.
            bool swapped;
            do
            {
                swapped = false;
                for (int i = 0; i < list.Count - 1; i++)
                {
                    if (string.Compare(list[i], list[i + 1], StringComparison.OrdinalIgnoreCase) > 0)
                    {
                        string temp = list[i];
                        list[i] = list[i + 1];
                        list[i + 1] = temp;
                        swapped = true;
                    }
                }
            } while (swapped);
        }

        private void button_Submit_Click(object sender, EventArgs e)
        {

            
            BubbleSort(callNumbers);

            // Getting the user's input and split it into individual call numbers.
            string[] userInput = textBox_userInput.Text.Split(new char[] { ' ' }, StringSplitOptions.RemoveEmptyEntries);

            // This code will check if the user sorted correctly.
            bool isCorrect = CheckSorting(userInput);

            if (isCorrect)
            {
                score++;
                MessageBox.Show("Well-done! You have sorted the numbers correctly.");
            }
            else
            {
                MessageBox.Show("Please try again, incorrect sorting!.");
            }

            // Update the user on their score .
            labelScore.Text = $"Your points are: {score}";
        }

        private void button_back_Click(object sender, EventArgs e)
        {
            Startup Form = new Startup();
            Form.Show();
        }
    }
}



8.  Select the Start button to run the application.




