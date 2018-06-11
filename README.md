# MovieLibrary
Final project Object C# 

//Client side, client(prod) folder
//Window application

using Project_Part1_Andrea_Pinilla.logic;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.IO;
using System.Windows.Forms;
using System.Runtime.Serialization.Formatters.Binary;
using System.Runtime.Serialization;


namespace Project_Part1_Andrea_Pinilla.client
{
    public partial class MovieLibraryForm : Form
    {
        List<Movies> myMoviesList = new List<Movies>();
        int index;

        static String path = @"../../data/movieLibraryText.txt";
        static String binFilepath = @"../../data/listLAST.bin";

        public MovieLibraryForm()
        {
            InitializeComponent();
        }
        private void ValidateDigit(KeyPressEventArgs e)
        {
            if (!Char.IsDigit(e.KeyChar))
            {

                MessageBox.Show("Must be a digit only");
                e.Handled = true;

            }
        }
        private int ValidateNumbers(string input)
        {
            bool result = Int32.TryParse(input, out int tmpE);
            if (result)
            {
                return tmpE;
            }
            else
            {
                MessageBox.Show("Movie ID must be digit only");
                return tmpE = 0;
            }

        }
        private void ValidateHours(string text)
        {
            int tmpHrs = Convert.ToInt32(text);
            if (tmpHrs > 20)
            {
                MessageBox.Show("Please enter a real hour value");
            }
        }
        private bool ValidateMoviesNumbers(int id)
        {
            bool goodId = false;
            if (id >= 1 && id < 9999)
            {
                return goodId = true;
            }
            else
            {
                return goodId;
            }
        }
        private void buttonAdd_Click(object sender, EventArgs e)
        {
            this.textBoxId.Focus();
            Duration aDuration = new Duration();
            aDuration.Hours = Convert.ToInt32(textBoxHours.Text);
            aDuration.Minutes = Convert.ToInt32(textBoxMinutes.Text);

            Movies aMovie = new Movies();

            String input; int tmpMovieNum; String tmpMovieTitle; EnumGenre tmpGenre; String tmpCountry;
            int tmpYear; EnumDirector tmpDirector; EnumActor tmpActor; EnumLenguage tmpLenguage;
            EnumCCOnOff tmpCC; EnumCCLenguage tmpCCLen; EnumAwards tmpAward;

            input = textBoxId.Text;
            tmpMovieNum = ValidateNumbers(input);
            if (tmpMovieNum == 0)
            {
                MessageBox.Show("Please enter the movie ID again");
                this.textBoxId.Focus();
            }
            else
            {
                ValidateMoviesNumbers(tmpMovieNum);
                if (ValidateMoviesNumbers(tmpMovieNum) == false)
                {

                    MessageBox.Show("The Movie ID must be between 1 and 9999, please enter the account number again");
                    this.textBoxId.Focus();
                }
                else
                {
                    tmpMovieTitle = textBoxTitle.Text;
                    tmpGenre = (EnumGenre)comboBoxGenre.SelectedIndex;
                    tmpCountry = textBoxCountry.Text;
                    tmpYear = Convert.ToInt32(textBoxYear.Text);
                    tmpDirector = (EnumDirector)comboBoxDirector.SelectedIndex;
                    tmpActor = (EnumActor)comboBoxActor.SelectedIndex;
                    tmpLenguage = (EnumLenguage)comboBoxLenguage.SelectedIndex;
                    tmpCC = (EnumCCOnOff)comboBoxCloseCaption.SelectedIndex;
                    tmpCCLen = (EnumCCLenguage)comboBoxCCLeng.SelectedIndex;
                    tmpAward = (EnumAwards)comboBoxAward.SelectedIndex;

                    aMovie.MovieNumber = tmpMovieNum;
                    aMovie.MovieTitle = tmpMovieTitle;
                    aMovie.Duration = aDuration;
                    aMovie.Genre = tmpGenre;
                    aMovie.Country = tmpCountry;
                    aMovie.Year = tmpYear;
                    aMovie.Director = tmpDirector;
                    aMovie.Actor = tmpActor;
                    aMovie.Lenguage = tmpLenguage;
                    aMovie.CloseCaption = tmpCC;
                    aMovie.CcLenguage = tmpCCLen;
                    aMovie.Award = tmpAward;

                    this.myMoviesList.Add(aMovie);
                }
            }

        }
        private void buttonDisplay_Click(object sender, EventArgs e)
        {
            if (this.myMoviesList.Capacity != 0)
            {
                foreach (Movies element in this.myMoviesList)
                {
                    ListViewItem item = new ListViewItem(Convert.ToString(element.MovieNumber));

                    item.SubItems.Add(Convert.ToString(element.MovieTitle));
                    item.SubItems.Add(Convert.ToString(element.Duration));
                    item.SubItems.Add(Convert.ToString(element.Genre));
                    item.SubItems.Add(Convert.ToString(element.Country));
                    item.SubItems.Add(Convert.ToString(element.Year));
                    item.SubItems.Add(Convert.ToString(element.Director));
                    item.SubItems.Add(Convert.ToString(element.Actor));
                    item.SubItems.Add(Convert.ToString(element.Lenguage));
                    item.SubItems.Add(Convert.ToString(element.CloseCaption));
                    item.SubItems.Add(Convert.ToString(element.CcLenguage));
                    item.SubItems.Add(Convert.ToString(element.Award));


                    this.listViewMovies.Items.Add(item);
                }

            }
            else
            {
                MessageBox.Show("...NO DATA.....");
            }
        }
        private void buttonUpdate_Click(object sender, EventArgs e)
        {
            DialogResult result = MessageBox.Show("Are you sure that you want to update this record?", "UPDATE", MessageBoxButtons.YesNo, MessageBoxIcon.Question);

            if (result == DialogResult.Yes)
            {

                this.myMoviesList[index].MovieNumber = Convert.ToInt32(textBoxId.Text);
                this.myMoviesList[index].MovieTitle = textBoxTitle.Text;
                this.myMoviesList[index].Duration.Hours = Convert.ToInt32(textBoxHours.Text);
                this.myMoviesList[index].Duration.Minutes = Convert.ToInt32(textBoxMinutes.Text);
                this.myMoviesList[index].Genre = (EnumGenre)comboBoxGenre.SelectedIndex;
                this.myMoviesList[index].Country = textBoxCountry.Text;
                this.myMoviesList[index].Year = Convert.ToInt32(textBoxYear.Text);
                this.myMoviesList[index].Director = (EnumDirector)comboBoxDirector.SelectedIndex;
                this.myMoviesList[index].Actor = (EnumActor)comboBoxActor.SelectedIndex;
                this.myMoviesList[index].Lenguage = (EnumLenguage)comboBoxLenguage.SelectedIndex;
                this.myMoviesList[index].CloseCaption = (EnumCCOnOff)comboBoxCloseCaption.SelectedIndex;
                this.myMoviesList[index].CcLenguage = (EnumCCLenguage)comboBoxCCLeng.SelectedIndex;
                this.myMoviesList[index].Award = (EnumAwards)comboBoxAward.SelectedIndex;

                MessageBox.Show("Record index: " + index + " updated");
                this.listBoxMovieInfo.Items.Clear();
                this.listViewMovies.Items.Clear();

               
                foreach (Movies element in myMoviesList)
                {
                    ListViewItem item = new ListViewItem(Convert.ToString(element.MovieNumber));
                    item.SubItems.Add(Convert.ToString(element.MovieTitle));
                    item.SubItems.Add(Convert.ToString(element.Duration));
                    item.SubItems.Add(Convert.ToString(element.Genre));
                    item.SubItems.Add(Convert.ToString(element.Country));
                    item.SubItems.Add(Convert.ToString(element.Year));
                    item.SubItems.Add(Convert.ToString(element.Director));
                    item.SubItems.Add(Convert.ToString(element.Actor));
                    item.SubItems.Add(Convert.ToString(element.Lenguage));
                    item.SubItems.Add(Convert.ToString(element.CloseCaption));
                    item.SubItems.Add(Convert.ToString(element.CcLenguage));
                    item.SubItems.Add(Convert.ToString(element.Award));


                    this.listViewMovies.Items.Add(item);
                }
            }
            else { MessageBox.Show("Data not found"); }
        }

        private void buttonDelete_Click(object sender, EventArgs e)
        {
            if (listBoxMovieInfo.SelectedIndex >= 0)
            {
                myMoviesList.RemoveAt(listBoxMovieInfo.SelectedIndex);
            }
            if (listViewMovies.FocusedItem.Index >= 0)
            {
                myMoviesList.RemoveAt(listViewMovies.FocusedItem.Index);
            }
            this.listBoxMovieInfo.Items.Clear(); this.listViewMovies.Items.Clear();
            foreach (Movies anAccount in this.myMoviesList)
            {
                this.listBoxMovieInfo.Items.Add(anAccount);
            }

            foreach (Movies element in myMoviesList)
            {
                ListViewItem item = new ListViewItem(Convert.ToString(element.MovieNumber));
                item.SubItems.Add(Convert.ToString(element.MovieTitle));
                item.SubItems.Add(Convert.ToString(element.Duration));
                item.SubItems.Add(Convert.ToString(element.Genre));
                item.SubItems.Add(Convert.ToString(element.Country));
                item.SubItems.Add(Convert.ToString(element.Year));
                item.SubItems.Add(Convert.ToString(element.Director));
                item.SubItems.Add(Convert.ToString(element.Actor));
                item.SubItems.Add(Convert.ToString(element.Lenguage));
                item.SubItems.Add(Convert.ToString(element.CloseCaption));
                item.SubItems.Add(Convert.ToString(element.CcLenguage));
                item.SubItems.Add(Convert.ToString(element.Award));


                this.listViewMovies.Items.Add(item);
            }
        }
        private void buttonReset_Click(object sender, EventArgs e)
        {
            foreach (Control myControl in Controls)
            {
                if (myControl is TextBox)
                {
                    myControl.Text = "";
                }
                if (myControl is ComboBox)
                {
                    myControl.Text = Convert.ToString(comboBoxGenre.Items[0]);
                    myControl.Text = Convert.ToString(comboBoxAward.Items[0]);
                    myControl.Text = Convert.ToString(comboBoxLenguage.Items[0]);
                    myControl.Text = Convert.ToString(comboBoxCloseCaption.Items[0]);
                    myControl.Text = Convert.ToString(comboBoxCCLeng.Items[0]);
                    myControl.Text = Convert.ToString(comboBoxDirector.Items[0]);
                    myControl.Text = Convert.ToString(comboBoxActor.Items[0]);

                }
                if (myControl is ListBox)
                {
                    listBoxMovieInfo.Items.Clear();
                }
                this.textBoxId.Enabled = true;
                this.listViewMovies.Items.Clear();
                this.textBoxId.Focus();
            }
        }

        private void buttonSearch_Click(object sender, EventArgs e)
        {
            bool found = false;
            Movies someMovie = new Movies();
            foreach (Movies item in this.myMoviesList)
            {
                if (item.MovieNumber == Convert.ToInt32(textBoxSearch.Text))
                {
                    found = true;
                    someMovie = item;
                }
            }
            if (found == true)
            {
                MessageBox.Show("Movie: " + someMovie + " found!!");
            }
            else { MessageBox.Show("Movie NOT found "); }
        }

        private void buttonExit_Click(object sender, EventArgs e)
        {
            this.Close();
        }

        private void listBoxMovieInfo_SelectedIndexChanged(object sender, EventArgs e)
        {
            index = listBoxMovieInfo.SelectedIndex;
            MessageBox.Show("Movie index " + index + " selected");
            textBoxId.Text = Convert.ToString(this.myMoviesList[index].MovieNumber);
            textBoxTitle.Text = this.myMoviesList[index].MovieTitle;
            textBoxHours.Text = Convert.ToString(this.myMoviesList[index].Duration.Hours);
            textBoxMinutes.Text = Convert.ToString(this.myMoviesList[index].Duration.Minutes);
            comboBoxGenre.Text = Convert.ToString(this.myMoviesList[index].Genre);
            textBoxCountry.Text = this.myMoviesList[index].Country;
            textBoxYear.Text = Convert.ToString(this.myMoviesList[index].Year);
            textBoxDirector.Text = Convert.ToString(this.myMoviesList[index].Director);
            textBoxActor.Text = Convert.ToString(this.myMoviesList[index].Actor);
            comboBoxLenguage.Text = Convert.ToString(this.myMoviesList[index].Lenguage);
            comboBoxCloseCaption.Text = Convert.ToString(this.myMoviesList[index].CloseCaption);
            comboBoxCCLeng.Text = Convert.ToString(this.myMoviesList[index].CcLenguage);
            comboBoxAward.Text = Convert.ToString(this.myMoviesList[index].Award);

            textBoxId.Enabled = false;
        }

        private void MovieLibraryForm_Load(object sender, EventArgs e)
        {

            foreach (EnumGenre element in Enum.GetValues(typeof(EnumGenre)))
            {
                comboBoxGenre.Items.Add(element);
                comboBoxGenre.Text = Convert.ToString(comboBoxGenre.Items[0]);
            }


            foreach (EnumLenguage element in Enum.GetValues(typeof(EnumLenguage)))
            {
                comboBoxLenguage.Items.Add(element);
                comboBoxLenguage.Text = Convert.ToString(comboBoxLenguage.Items[0]);
            }


            foreach (EnumCCOnOff element in Enum.GetValues(typeof(EnumCCOnOff)))
            {
                comboBoxCloseCaption.Items.Add(element);
                comboBoxCloseCaption.Text = Convert.ToString(comboBoxCloseCaption.Items[0]);
            }


            foreach (EnumCCLenguage element in Enum.GetValues(typeof(EnumCCLenguage)))
            {
                comboBoxCCLeng.Items.Add(element);
                comboBoxCCLeng.Text = Convert.ToString(comboBoxCCLeng.Items[0]);
            }


            foreach (EnumAwards element in Enum.GetValues(typeof(EnumAwards)))
            {
                comboBoxAward.Items.Add(element);
                comboBoxAward.Text = Convert.ToString(comboBoxAward.Items[0]);
            }


            foreach (EnumDirector element in Enum.GetValues(typeof(EnumDirector)))
            {
                comboBoxDirector.Items.Add(element);
                comboBoxDirector.Text = Convert.ToString(comboBoxDirector.Items[0]);
            }


            foreach (EnumActor element in Enum.GetValues(typeof(EnumActor)))
            {
                comboBoxActor.Items.Add(element);
                comboBoxActor.Text = Convert.ToString(comboBoxActor.Items[0]);
            }

        }

        private void textBoxId_KeyPress(object sender, KeyPressEventArgs e)
        {
            ValidateDigit(e);
        }

        private void textBoxHours_TextChanged(object sender, EventArgs e)
        {

        }



        private void textBoxMinutes_TextChanged(object sender, EventArgs e)
        {
            //ValidateMinutes(Convert.ToInt32(textBoxMinutes.Text));
        }

        private void ValidateMinutes(int v)
        {
            int tmpMin = v;
            if (tmpMin > 59)
            {
                MessageBox.Show("Minute input not valid, enter a value max. 59");
                textBoxMinutes.Text = "";
            }
        }

        private void textBoxTitle_TextChanged(object sender, EventArgs e)
        {

        }

        private void textBoxHours_KeyPress(object sender, KeyPressEventArgs e)
        {
            ValidateDigit(e);
        }

        private void textBoxMinutes_KeyPress(object sender, KeyPressEventArgs e)
        {
            ValidateDigit(e);

        }

        private void listViewMovies_SelectedIndexChanged(object sender, EventArgs e)
        {
            index = listViewMovies.FocusedItem.Index;

            textBoxId.Text = Convert.ToString(this.myMoviesList[index].MovieNumber);
            textBoxTitle.Text = this.myMoviesList[index].MovieTitle;
            textBoxHours.Text = Convert.ToString(this.myMoviesList[index].Duration.Hours);
            textBoxMinutes.Text = Convert.ToString(this.myMoviesList[index].Duration.Minutes);
            comboBoxGenre.Text = Convert.ToString(this.myMoviesList[index].Genre);
            textBoxCountry.Text = this.myMoviesList[index].Country;
            textBoxYear.Text = Convert.ToString(this.myMoviesList[index].Year);
            comboBoxDirector.Text = Convert.ToString(this.myMoviesList[index].Director);
            comboBoxActor.Text = Convert.ToString(this.myMoviesList[index].Actor);
            comboBoxLenguage.Text = Convert.ToString(this.myMoviesList[index].Lenguage);
            comboBoxCloseCaption.Text = Convert.ToString(this.myMoviesList[index].CloseCaption);
            comboBoxCCLeng.Text = Convert.ToString(this.myMoviesList[index].CcLenguage);
            comboBoxAward.Text = Convert.ToString(this.myMoviesList[index].Award);

            textBoxId.Enabled = false;
        }

        private void buttonWriteTxt_Click(object sender, EventArgs e)
        {
            using (StreamWriter writer = File.CreateText(path))
            {
                foreach (Movies aMovie in this.myMoviesList)
                {

                    writer.WriteLine(aMovie.MovieNumber + "|" + aMovie.MovieTitle + "|" + aMovie.Duration + "|"
                                    + aMovie.Genre + "|" + aMovie.Country + "|" + aMovie.Year + "|" + aMovie.Director + "|"
                                    + aMovie.Actor + "|" + aMovie.Lenguage + "|" + aMovie.CloseCaption + "|" + aMovie.CcLenguage + "|" + aMovie.Award);
                }
                //writer.Close();
            }
        }

        private void buttonReadTxt_Click(object sender, EventArgs e)
        {

            myMoviesList.Clear();
            this.listViewMovies.Items.Clear();

            StreamReader reader = new StreamReader(path);
            String line1 = null;
            String time = null;
            while ((line1 = reader.ReadLine()) != null)
            {
                Movies aMovie = new Movies();

                String[] tokens = line1.Split('|');

                aMovie.MovieNumber = Convert.ToInt32(tokens[0]);
                aMovie.MovieTitle = tokens[1];
                time = tokens[2];

                String[] tokensTime = time.Split(':');
                Duration aTime = new Duration();
                aTime.Hours = Convert.ToInt32(tokensTime[0]);
                aTime.Minutes = Convert.ToInt32(tokensTime[1]);
                aMovie.Duration = aTime;
                aMovie.Genre = (EnumGenre)Enum.Parse(typeof(EnumGenre), tokens[3]);
                aMovie.Country = tokens[4];
                aMovie.Year = Convert.ToInt32(tokens[5]);
                aMovie.Director = (EnumDirector)Enum.Parse(typeof(EnumDirector), tokens[6]);
                aMovie.Actor = (EnumActor)Enum.Parse(typeof(EnumActor), tokens[7]);
                aMovie.Lenguage = (EnumLenguage)Enum.Parse(typeof(EnumLenguage), tokens[8]);
                aMovie.CloseCaption = (EnumCCOnOff)Enum.Parse(typeof(EnumCCOnOff), tokens[9]);
                aMovie.CcLenguage = (EnumCCLenguage)Enum.Parse(typeof(EnumCCLenguage), tokens[10]);
                aMovie.Award = (EnumAwards)Enum.Parse(typeof(EnumAwards), tokens[11]);
                myMoviesList.Add(aMovie);

            }
            
            foreach (Movies item in myMoviesList)
            {
                //this.listViewMovies.Items.Add(item.ToString());

                ListViewItem element = new ListViewItem(Convert.ToString(item.MovieNumber));

                element.SubItems.Add(Convert.ToString(item.MovieTitle));
                element.SubItems.Add(Convert.ToString(item.Duration));
                element.SubItems.Add(Convert.ToString(item.Genre));
                element.SubItems.Add(Convert.ToString(item.Country));
                element.SubItems.Add(Convert.ToString(item.Year));
                element.SubItems.Add(Convert.ToString(item.Director));
                element.SubItems.Add(Convert.ToString(item.Actor));
                element.SubItems.Add(Convert.ToString(item.Lenguage));
                element.SubItems.Add(Convert.ToString(item.CloseCaption));
                element.SubItems.Add(Convert.ToString(item.CcLenguage));
                element.SubItems.Add(Convert.ToString(item.Award));

                this.listViewMovies.Items.Add(element);
            }
            reader.Close();
        }

        private void textBoxYear_KeyPress(object sender, KeyPressEventArgs e)
        {
            ValidateDigit(e);
        }

        private void buttonWriteToBinFIle_Click(object sender, EventArgs e)
        {
            FileStream fs = new FileStream(binFilepath, FileMode.OpenOrCreate, FileAccess.Write);
            BinaryFormatter writer = new BinaryFormatter();
            writer.Serialize(fs, myMoviesList);
            fs.Close();
        }

        private void buttonReadFromBinFile_Click(object sender, EventArgs e)
        {
            if (File.Exists(binFilepath))
            {
                FileStream fs = new FileStream(binFilepath, FileMode.Open, FileAccess.Read);
                BinaryFormatter bin = new BinaryFormatter();
                myMoviesList = (List<Movies>)bin.Deserialize(fs);

                fs.Close();
            }
            foreach (Movies item in myMoviesList)
            {
                ListViewItem element = new ListViewItem(Convert.ToString(item.MovieNumber));

                element.SubItems.Add(Convert.ToString(item.MovieTitle));
                element.SubItems.Add(Convert.ToString(item.Duration));
                element.SubItems.Add(Convert.ToString(item.Genre));
                element.SubItems.Add(Convert.ToString(item.Country));
                element.SubItems.Add(Convert.ToString(item.Year));
                element.SubItems.Add(Convert.ToString(item.Director));
                element.SubItems.Add(Convert.ToString(item.Actor));
                element.SubItems.Add(Convert.ToString(item.Lenguage));
                element.SubItems.Add(Convert.ToString(item.CloseCaption));
                element.SubItems.Add(Convert.ToString(item.CcLenguage));
                element.SubItems.Add(Convert.ToString(item.Award));

                this.listViewMovies.Items.Add(element);

            }

        }
    }
}
