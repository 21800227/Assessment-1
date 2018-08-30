# Assessment-1
Programming assessment from 2018
//Name: Rhiannon Williams
//ID: 21800227
//Date:8/8/18
//Version: 1.0
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Assessment01
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        //Customer Details
        string s_FName, s_LName, s_Address, s_City, s_Country;
        //Card Details
        string s_CardType, s_CardName, s_CardMonth, s_CardNumber, s_CardYear;
        //Product Information
        int i_ShoesU = 25, i_ShoesQ, i_ShoesT;
        int i_JacketU = 50, i_JacketQ, i_JacketT;
        int i_GlovesU = 10, i_GlovesQ, i_GlovesT;
        int i_BeanieU = 10, i_BeanieQ, i_BeanieT;
        //Other Fees
        double d_DiscountU, d_DiscountT, d_GST = 0.15;
        //total values
        int i_ProductValues;
        double d_Gross, /*Value before GST*/ d_Total;//value after GST
        //Summary Message
        string s_Message;

        private void Form1_Load(object sender, EventArgs e)
        {
            //Expirey Year Field Population
            int i_Year = DateTime.Now.Year;
            int i_EndYear = i_Year + 15;
            while (i_Year <= i_EndYear)
            {
                cmbYear.Items.Add(i_Year);
                i_Year++;
            }

            //Check if everything is unchecked or deselscted
            radCash.Checked = false;
            radBank.Checked = false;
            radCredit.Checked = false;
            chkDiscount.Checked = false;
            chkGST.Checked = false;
            chkShoes.Checked = false;
            chkJackets.Checked = false;
            chkBeanies.Checked = false;
            chkGloves.Checked = false;
            grpCardInfo.Enabled = false;
        }
       
        private void GetUserInput()
        {
            //Collect user input
            s_FName = Convert.ToString(txtFirstName.Text);
            s_LName = Convert.ToString(txtLastName.Text);
            s_Address = Convert.ToString(txtAddress.Text);
            s_City = Convert.ToString(txtCity.Text);
            s_Country = Convert.ToString(cmbCountry.Text);

            if (grpCardInfo.Enabled == true)
            {
                s_CardType = Convert.ToString(cmbCardType.Text);
                s_CardNumber = Convert.ToString(txtCardNumber.Text);
                s_CardName = Convert.ToString(txtCardName.Text);
                s_CardMonth = Convert.ToString(cmbMonth.Text);
                s_CardYear = Convert.ToString(cmbYear.Text);
            }

            if (chkShoes.Checked) { i_ShoesQ = Convert.ToInt32(txtShoes.Text); }
            if (chkJackets.Checked) { i_JacketQ = Convert.ToInt32(txtJacket.Text); }
            if (chkGloves.Checked) { i_GlovesQ = Convert.ToInt32(txtGloves.Text); }
            if (chkBeanies.Checked) { i_BeanieQ = Convert.ToInt32(txtBeanies.Text); }

            if (chkDiscount.Checked) { d_DiscountU = Convert.ToInt32(txtDiscount.Text); }
            
        }
        
        private void Calculations()
        {
            //Calculate Cost of selected Products
            if (chkShoes.Checked) { i_ShoesT = i_ShoesU * i_ShoesQ; }
            else { i_ShoesT = 0; }
            if (chkJackets.Checked) { i_JacketT = i_JacketU * i_JacketQ; }
            else { i_JacketT = 0; }
            if (chkBeanies.Checked) { i_BeanieT = i_BeanieU * i_BeanieQ; }
            else { i_BeanieT = 0; }
            if (chkGloves.Checked) { i_GlovesT = i_GlovesU * i_GlovesQ; }
            else { i_GlovesT = 0; }
           
            //Total Product Values
            i_ProductValues = i_ShoesT + i_JacketT + i_BeanieT + i_GlovesT;

            //Discounts, If any
            if (d_DiscountU > 50)
            {
                MessageBox.Show("Please enter a valid discount ammount.");
                txtDiscount.Text = "1";
                if (chkDiscount.Checked) { d_DiscountU = Convert.ToInt32(txtDiscount.Text); }
            }

            if (chkDiscount.Checked) { d_DiscountT = i_ProductValues * d_DiscountU / 100; }
            else { d_DiscountT = 0; }
            lblDiscount.Text = d_DiscountT.ToString();
            
            //GST calculations
            if (chkGST.Checked) { d_Gross = i_ProductValues * d_GST; }
            else { d_Gross = 0; }
            lblGST.Text = d_Gross.ToString();

            //Calculate Final Ammount
            d_Total = i_ProductValues - d_DiscountT + d_Gross;
            if( d_Total < 50)
            {
                MessageBox.Show("Sorry, Credit is not available for values less than $50.");
                radCredit.Enabled = false;
                grpCardInfo.Enabled = false;
                radCash.Enabled = true;
            }
            else if (d_Total >= 50 && d_Total <= 1200)
            {
                radCash.Enabled = true;
                radCredit.Enabled = true;
            }
            else if ( d_Total > 1200)
            {
                MessageBox.Show("Sorry, Cash is not available for values more than $1,200. ");
                radCash.Enabled = false;
                radCredit.Enabled = true;
            }
        }

        private void SummaryMessage()
        {
            s_Message = "Summary" + "\n" + "\n" + "Customer Details" +"\n" + "Name: " + s_FName + " " + s_LName + "\n" + "Address: " + s_Address + "\n" + "City: " + s_City + "\n" + "Country: " + s_Country + "\n";
            if (radCredit.Checked) { s_Message = s_Message + "\n" + "Card Details" + "\n" + "Card Type: " + s_CardType + "\n" + "Card Number: " + s_CardNumber + "\n" + "Card Name: " + s_CardName + "\n" + "Expiry Date: " + s_CardMonth + " " + s_CardYear + "\n"; }
            s_Message = s_Message + "\n" + "Product Selection:" + "\n";
            if (chkShoes.Checked) { s_Message = s_Message + "Shoes Price: $" + i_ShoesU + "\n" + "Purchace Ammount: " + i_ShoesQ + "\n"; }
            if (chkJackets.Checked) { s_Message = s_Message + "Jacket Price: $" + i_JacketU + "\n" + "Purchace Ammount: " + i_JacketQ + "\n"; }
            if (chkBeanies.Checked) { s_Message = s_Message + "Beanie Price: $" + i_BeanieU + "\n" + "Purchace Ammount: " + i_BeanieQ + "\n"; }
            if (chkGloves.Checked) { s_Message = s_Message + "Gloves Price: $" + i_GlovesU + "\n" + "Purchace Ammount: " + i_GlovesQ + "\n"; }
            if (chkDiscount.Checked) { s_Message = s_Message + "\n" + "Discount" + "\n" + "Discount %: " + d_DiscountU + "\n" + "Discount ammount: " + d_DiscountT + "\n"; }
            if (chkGST.Checked) { s_Message = s_Message + "\n" + "GST" + "\n" + "GST: " + d_GST + "\n" + "GST Ammount: " + d_Gross + "\n"; }
            s_Message = s_Message + "\n" + "FINAL AMMOUNT" + "\n" + "Total: " + d_Total;
            MessageBox.Show(s_Message );
        }

        private void radCredit_CheckedChanged(object sender, EventArgs e)
        {
            //Enable/Disable Card Info Group
            if (radCredit.Checked == true)
            { grpCardInfo.Enabled = true; }
            else { grpCardInfo.Enabled = false; }
        }

        private void btnUpdate_Click(object sender, EventArgs e)
        {
            GetUserInput();

            Calculations();

            lblFinalAmount.Text = d_Total.ToString();
        }

        private void btnSummary_Click(object sender, EventArgs e)
        {
            //Check user Input
            if (txtFirstName.Text == string.Empty)
            { MessageBox.Show("Please Enter your First Name."); return; }
            if (txtLastName.Text == string.Empty)
            { MessageBox.Show("Please enter your Last Name"); return; }
            if (radCredit.Checked == true)
            {
                if (txtAddress.Text == string.Empty)
                { MessageBox.Show("Please enter your street number and name."); return; }
                if (txtCity.Text == string.Empty)
                { MessageBox.Show("Please enter the City/Town you live in"); return; }
                if (cmbCountry.Text == string.Empty)
                { MessageBox.Show("Please Select a county"); return; }

                if (cmbCardType.Text == string.Empty)
                { MessageBox.Show("Please select a card type"); return; }
                if (txtCardName.Text == string.Empty)
                { MessageBox.Show("Please Enter the name on the card"); return; }
                if (txtCardNumber.Text == string.Empty)
                { MessageBox.Show("Please Enter a card number"); return; }
                if (cmbMonth.Text == string.Empty)
                { MessageBox.Show("Please select an expiery month"); return; }
                if (cmbYear.Text == string.Empty)
                { MessageBox.Show("Please select an expiery year"); return; }
            }
            GetUserInput();

            Calculations();

            SummaryMessage();
        }

        private void btnReset_Click(object sender, EventArgs e)
        {
            radCash.Checked = false;
            radBank.Checked = false;
            radCredit.Checked = false;
            chkDiscount.Checked = false;
            chkGST.Checked = false;
            chkShoes.Checked = false;
            chkJackets.Checked = false;
            chkBeanies.Checked = false;
            chkGloves.Checked = false;
            grpCardInfo.Enabled = false;
            radCash.Enabled = true;
            radCredit.Enabled = true;

            txtLastName.Text = "";
            txtFirstName.Text = "";
            txtAddress.Text = "";
            txtCity.Text = "";
            txtCardNumber.Text = "";
            txtCardName.Text = "";
            txtShoes.Text = "";
            txtJacket.Text = "";
            txtGloves.Text = "";
            txtBeanies.Text = "";
            txtDiscount.Text = "";

            lblDiscount.Text = "$";
            lblGST.Text = "$";
            lblFinalAmount.Text = "$";

            cmbCardType.ResetText();
            cmbCountry.ResetText();
            cmbMonth.ResetText();
            cmbYear.ResetText();

            txtFirstName.Focus();
        }

        private void btnExit_Click(object sender, EventArgs e)
        {
            this.Close();
        }
    }
}
