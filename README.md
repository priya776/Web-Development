using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WebProject
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            // Set background image
            this.BackgroundImage = Image.FromFile("C:\\Users\\Priya\\bg.png");
            // Set default texts
            comboBoxFrom.Text = "Select Unit"; // Default text for From ComboBox
            comboBoxTo.Text = "Select Unit";   // Default text for To ComboBox
            //txtInput.Text = "Enter Value";     // Default text for Input TextBox

                        
            // Add Conversion types to Category ComboBox
            comboBoxCategory.Items.AddRange(new string[] { "Length", "Weight", "Temperature" });

        }

        private void comboBoxCategory_SelectedIndexChanged(object sender, EventArgs e)
        {
            string selectedCategory = comboBoxCategory.SelectedItem.ToString();

            // Clear unit ComboBoxes
            comboBoxTo.Items.Clear();
            comboBoxFrom.Items.Clear();

            // Populate unit ComboBoxes based on selected category
            switch (selectedCategory)
            {
                case "Length":
                    comboBoxTo.Items.AddRange(new string[] { "Centimetres", "Metres", "Kilometres", "Inches", "Miles"});
                    comboBoxFrom.Items.AddRange(new string[] { "Centimetres", "Metres", "Kilometres", "Inches", "Miles" });
                    break;
                case "Weight":
                    comboBoxTo.Items.AddRange(new string[] { "Grams", "Kilograms", "Pounds" });
                    comboBoxFrom.Items.AddRange(new string[] { "Grams", "Kilograms", "Pounds" });
                    break;
                case "Temperature":
                    comboBoxTo.Items.AddRange(new string[] { "Celsius", "Fahrenheit", "Kelvin" });
                    comboBoxFrom.Items.AddRange(new string[] { "Celsius", "Fahrenheit", "Kelvin" });
                    break;
                default: break;

            }

            // Clear Input and output fields when category is changed
              txtOutput.Clear();
              txtInput.Clear();     
            // Set default texts
            comboBoxFrom.Text = "Select Unit"; // Reset text for From ComboBox
            comboBoxTo.Text = "Select Unit";   // Reset text for To ComboBox
            
            
        }

        private void btnConvert_Click(object sender, EventArgs e)
        {
            double input = 0   ;
            double result = 0;

            // Validate the input is a number
            if (!double.TryParse(txtInput.Text, out input))
            {
                MessageBox.Show("Please enter a valid number.");
                return;
            }

            // Get the selected units and category
            string fromUnit = comboBoxFrom.SelectedItem?.ToString();
            string toUnit = comboBoxTo.SelectedItem?.ToString();

            string selectedCategory = comboBoxCategory.SelectedItem?.ToString();

            if (fromUnit == null || toUnit == null || selectedCategory == null)
            {
                MessageBox.Show("Please select both 'From' and 'To' units and a conversion category.");
                return;
            }

            // Perform the conversion
            result = ConvertUnits(input, fromUnit, toUnit, selectedCategory);

            // Display the result
            txtOutput.Text = result.ToString();
        }


        private double ConvertUnits(double value, string fromUnit, string toUnit, string category)
        {
            switch (category)
            {
                case "Length":
                    return ConvertLength(value, fromUnit, toUnit);
                case "Weight":
                    return ConvertWeight(value, fromUnit, toUnit);
                case "Temperature":
                    return ConvertTemperature(value, fromUnit, toUnit);
                default:
                    return value;
            }
        }

        // Length conversion logic
        private double ConvertLength(double value, string fromUnit, string toUnit)
        {
            // Convert 'from' unit to meters first (base unit)
            double valueInMeters = value;
            switch (fromUnit)
            {
                case "Centimetres": valueInMeters = value / 100; break;   // 1 centimetre = 1/100 meters
                case "Metres": valueInMeters = value; break;              // base unit (no conversion needed)
                case "Kilometres": valueInMeters = value * 1000; break;   // 1 kilometre = 1000 meters
                case "Inches": valueInMeters = value * 0.0254; break;     // 1 inch = 0.0254 meters
                case "Miles": valueInMeters = value * 1609.34; break;     // 1 mile = 1609.34 meters
                default: return -1; // Invalid 'from' unit
            }

           // Convert from meters to the 'to' unit
           switch (toUnit)
            {
                case "Centimetres": return valueInMeters * 100; 
                case "Metres": return valueInMeters;
                case "Kilometres": return valueInMeters / 1000;
                case "Inches": return valueInMeters / 0.0254;
                case "Miles": return valueInMeters / 1609.34;
                default: return value;
            }
        }


        // Weight conversion logic
        private double ConvertWeight(double value, string fromUnit, string toUnit)
        {
            // Convert 'from' unit to grams first (base unit)
            double valueInGrams = value;
            switch (fromUnit)
            {
                case "Grams": valueInGrams = value; break;
                case "Kilograms": valueInGrams = value * 1000; break;
                case "Pounds": valueInGrams = value * 453.592; break;
            }

            // Convert from grams to the 'to' unit
            switch (toUnit)
            {
                case "Grams": return valueInGrams;
                case "Kilograms": return valueInGrams / 1000;
                case "Pounds": return valueInGrams / 453.592;
                default: return value;
            }
        }

        // Temperature conversion logic
        private double ConvertTemperature(double value, string fromUnit, string toUnit)
        {
            double tempInCelsius = value;

            // Convert 'from' unit to Celsius first (base unit)
            switch (fromUnit)
            {
                case "Celsius": tempInCelsius = value; break;
                case "Fahrenheit": tempInCelsius = (value - 32) * 5 / 9; break;
                case "Kelvin": tempInCelsius = value - 273.15; break;
            }

            // Convert from Celsius to the 'to' unit
            switch (toUnit)
            {
                case "Celsius": return tempInCelsius;
                case "Fahrenheit": return (tempInCelsius * 9 / 5) + 32;
                case "Kelvin": return tempInCelsius + 273.15;
                default: return value;
            }
        }

        private void txtInput_TextChanged(object sender, EventArgs e)
        {
           
        }
    }

 }



