# Ice-Cream-Parlour
# login.vb
Public Class login
    Dim conn As New OleDb.OleDbConnection
    Dim cmd As OleDb.OleDbCommand
    Dim da As OleDb.OleDbDataAdapter
    Dim dt As DataTable


    Private Sub Form1_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load
        conn.ConnectionString = "Provider=Microsoft.ACE.OLEDB.12.0;Data Source=C:\Users\HP\Documents\mydb.accdb"
        If conn.State = ConnectionState.Closed Then
            conn.Open()


        End If
        Dim sql As String = "select * from AdminLogin"
        da = New OleDb.OleDbDataAdapter
        If conn.State = ConnectionState.Closed Then
            conn.Open()

        End If

        cmd.CommandText = "select * from AdminLogin"
        cmd = New OleDb.OleDbCommand
        dt = New DataTable


    End Sub

    ' 1 page login button-> button1

    Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button1.Click

        cmd.Connection = conn
        dt.Clear()
        da.SelectCommand = cmd
        da.Fill(dt)
        If TextBox1.Text = "" Or TextBox2.Text = "" Then
            MsgBox("Please enter username and password")
            'textbox2=user name textbox1=password
        ElseIf dt.Rows(0).Item(0).ToString = TextBox2.Text And dt.Rows(0).Item(1).ToString = TextBox1.Text Then

            Billing.Show()
            TextBox1.Clear()
            TextBox2.Clear()

            Me.Hide()



        Else
            MsgBox("Please enter valid username and password")
        End If

    End Sub

   


   
End Class
# Billing.vb
Public Class Billing
    Dim small As Integer
    Dim large As Integer
    Dim medium As Integer
    Dim selectedPrice As Integer
    Dim selectedItem As String
    Dim t As Integer
    Dim conn As New OleDb.OleDbConnection
    Dim cmd As OleDb.OleDbCommand
    Dim da As OleDb.OleDbDataAdapter
    Dim dt As DataTable

    ' Dim mangoPiece As Integer
    ' Dim orangePiece As Integer
    ' Dim vanilaPiece As Integer
    ' Dim chocolatePiece As Integer
  



    Private Sub Form1_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load
        'TODO: This line of code loads data into the 'MydbDataSet5.Stock' table. You can move, or remove it, as needed.
        Me.StockTableAdapter.Fill(Me.MydbDataSet5.Stock)

        conn.ConnectionString = "Provider=Microsoft.ACE.OLEDB.12.0;Data Source=C:\Users\HP\Documents\mydb.accdb"
        If conn.State = ConnectionState.Closed Then
            conn.Open()

        End If

        da = New OleDb.OleDbDataAdapter
        cmd = New OleDb.OleDbCommand
        dt = New DataTable


        cmd.CommandText = "select * from PriceTable"
        cmd.Connection = conn
        dt.Clear()
        da.SelectCommand = cmd
        da.Fill(dt)


        small = dt.Rows(0).Item(0)
        medium = dt.Rows(0).Item(1)
        large = dt.Rows(0).Item(2)
        'When form open it update data in price lables 
        sPrice.Text = small
        mPrice.Text = medium
        lPrice.Text = large





    End Sub

    ' billing page Button 1= Add to your order button
    Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button1.Click

        If conn.State = ConnectionState.Closed Then
            conn.Open()

        End If
        'connection to price table
        cmd.CommandText = "select * from PriceTable"
        cmd.Connection = conn
        dt.Clear()
        da.SelectCommand = cmd
        da.Fill(dt)

        small = dt.Rows(0).Item(0)
        medium = dt.Rows(0).Item(1)
        large = dt.Rows(0).Item(2)

        'Radio 1= small   Radio 2= medium    Radio 3= large   ComboBox1=flaver   ComboBox2 quantity
        If (RadioButton1.Checked = False And RadioButton2.Checked = False And RadioButton3.Checked = False) Or (ComboBox1.Text = "" Or TextBox2.Text = "") Then
            MsgBox("Please Enter Order Details")
        Else





            Dim sql As String
            'textbox 2 = Quantity
            If RadioButton1.Checked Then
                selectedItem = "Small"
                selectedPrice = small
                totalPrice.Text = CInt(TextBox2.Text) * selectedPrice

            End If


            If RadioButton2.Checked Then
                selectedItem = "Medium"
                selectedPrice = medium
                totalPrice.Text = CInt(TextBox2.Text) * selectedPrice

            End If

            If RadioButton3.Checked Then
                selectedItem = "Large"
                selectedPrice = large
                totalPrice.Text = CInt(TextBox2.Text) * selectedPrice

            End If

            sql = "insert into dataTable values('" & ComboBox1.Text & "'," & CInt(TextBox2.Text) & ",'" & selectedItem & "'," & totalPrice.Text & " )"
            cmd = New OleDb.OleDbCommand(sql, conn)
            t = cmd.ExecuteNonQuery()

        End If


    End Sub

    '  Button 5= set price

    Private Sub Button5_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button5.Click
        Settings.Show()
        Me.Close()

    End Sub

    ' Button 3= log out
    Private Sub Button3_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button3.Click
        login.Show()
        Me.Close()


    End Sub

    'Button 4= check database
    Private Sub Button4_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button4.Click
        Form1.Show()
        Me.Hide()

    End Sub


   
End Class
# Settings.vb

    Private Sub Settings_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load
        conn.ConnectionString = "Provider=Microsoft.ACE.OLEDB.12.0;Data Source=C:\Users\HP\Documents\mydb.accdb"
        If conn.State = ConnectionState.Closed Then
            conn.Open()

        End If

        da = New OleDb.OleDbDataAdapter
        cmd = New OleDb.OleDbCommand
        dt = New DataTable

    End Sub

    'lable 5= back Button
    Private Sub Label5_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Label5.Click
        Billing.Show()
        Me.Hide()

    End Sub
    'Button 2 = Add Button
    Private Sub Button2_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles PriceBtn.Click
        Dim cmd1 As String = ""
        If conn.State = ConnectionState.Closed Then
            conn.Open()

        End If
        'Text box 1 = Enter price 
        Dim t As Integer
        If ComboBox1.Text = "" Or TextBox1.Text = "" Then
            MsgBox("Item and price Box cannot Be Empty")
        Else


            If ComboBox1.Text = "Small" Then

                cmd1 = "update PriceTable set Small=" & CInt(TextBox1.Text) & " "
                cmd = New OleDb.OleDbCommand(cmd1, conn)
                t = cmd.ExecuteNonQuery()
                MsgBox("Price Updated")
                TextBox1.Clear()



            ElseIf ComboBox1.Text = "Medium" Then

                cmd1 = "update PriceTable set Medium=" & CInt(TextBox1.Text) & " "
                cmd = New OleDb.OleDbCommand(cmd1, conn)
                t = cmd.ExecuteNonQuery()

                MsgBox("Price Updated")
                TextBox1.Clear()



            Else


                cmd1 = "update PriceTable set Large=" & CInt(TextBox1.Text) & " "
                cmd = New OleDb.OleDbCommand(cmd1, conn)
                t = cmd.ExecuteNonQuery()
                MsgBox("Price Updated")
                TextBox1.Clear()

            End If

        End If

    End Sub

End Class

