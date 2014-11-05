nuradinahmed-.NET-
==================

attribute/ignore
class SalesCatering
	def initialize() #Declaring Module Level Constannts:
		@PRIMERIB_PRICE_Decimal = 25
		@CHICKEN_PRICE_Decimal = 18
		@PASTA_PRICE_Decimal = 12
		@OPENBAR_PRICE_Decimal = 25
		@WINEWITHDINNER_PRICE_Decimal = 8
	end
 #WE ARE ALSO DECLARING SOME VARIABLES AT MODULE LEVEL FOR SUMMARY INFORMATION
	def CalculateButton_Click(sender, e) #CALCULATE AND DISPLAY THE CURRENT AMOUNT DUE AND THEN ADD THAT TO TOTALS TO BE USED AT SUMMARY IN MODULE LEVEL #This would find the price for each selecte option, especially the food manu that is checked through radiobuttons at once.
		if PrimeRibRadioButton.Checked then
			FoodChoiceDecimal = @PRIMERIB_PRICE_Decimal
		elsif ChickenRadioButton.Checked then
			FoodChoiceDecimal = @CHICKEN_PRICE_Decimal
		elsif PastaRadioButton.Checked then
			FoodChoiceDecimal = @PASTA_PRICE_Decimal
		end #This is would also allow us to check one or more option in the checke box by selecting the type of environment _& #check either the 'Open Bar' Or 'Wine'.
		if OpenBarCheckBox.Checked then
			OpenBarDecimal = @OPENBAR_PRICE_Decimal
		end
		if WineCheckBox.Checked then
			WineWithDinnerDecimal = @WINEWITHDINNER_PRICE_Decimal
		end #NOW, WE ARE GOING TO CALCUATE THE PRICE BY THE NUMBER OF GUESTS AND THE DRINKS THAT HAVE BEEN SELECTED.
		begin
			@NumberofGuestsInteger = System::Int32.Parse(GuestsTextBox.Text)
			DrinksDecimal = (OpenBarDecimal + WineWithDinnerDecimal)
			@AmountDueDecimal = (FoodChoiceDecimal + DrinksDecimal) * @NumberofGuestsInteger #Display the total 
			AmountDueTextBox.Text = @AmountDueDecimal.ToString("C") #We allow for change to happen in "NEW EVENT"
			OpenBarCheckBox.Enabled = false
			WineCheckBox.Enabled = false
			ClearButton.Enabled = true
			SummaryButton.Enabled = true
		rescue FormatException => NumberofGuestsException
			MessageBox.Show("Number of Guest Must be Numberic", "Data Entry Error", MessageBoxButtons.OK, MessageBoxIcon.Information)
		ensure
		end
	end

	def ClearButton_Click(sender, e)
		PrimeRibRadioButton.Checked = true #All others should be false. 
AmountDueTextBox		.Clear()
		.Focus()
GuestsTextBox		.Clear()
		.Focus() #Clear the current even on place and add the totals to the totals. #Confirm "clear" of the current even
		MessageString = "Clear the current Totals ?"
		@ReturnDialogResult = MessageBox.Show(MessageString, "Clear Event", MessageBoxButtons.YesNoCancel, MessageBoxIcon.Question, MessageBoxDefaultButton.Button3)
		if @ReturnDialogResult == DialogResult.Yes then #User said yes.
			self.ClearButton_Click(sender, e) #Clear the screen fields.
			GuestsTextBox.Clear()
			AmountDueTextBox.Clear()
		end #We add to the totals now 
		if @NumberofGuestsInteger > 0 then
			@TotalDollarAmountDecimal += @AmountDueDecimal
			@EventCountInteger += 1
		end # Reset totals for next event. # Claer the appropriate display items and enable Check Boxes.
OpenBarCheckBox		.Enabled = true
		.Checked = false
WineCheckBox		.Enabled = true
		.Checked = false
ClearButton.Enabled == false		SummaryButton.Enabled = true
	end

	def SummaryButton_Click(sender, e) # Calculate total dollar amounts and number of events.
		if @EventCountInteger > 0 then # Calculate the summary sales totals.
			@TotalDollarAmountDecimal += @AmountDueDecimal #Concatenate the message string.
			MessageString = "Number of Events:" + @EventCountInteger.ToString() + Environment.NewLine + Environment.NewLine + "Total Sales: " + @TotalDollarAmountDecimal.ToString("C")
			MessageBox.Show(MessageString, "Sales Summary", MessageBoxButtons.OK, MessageBoxIcon.Information)
		end
	end

	def ExitButton_Click(sender, e) #Close the Program
		self.Close()
	end
end
