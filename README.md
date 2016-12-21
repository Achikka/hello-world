Public Class Form1
    Dim Data1 As String
    Private Sub Form1_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load
        Button1.Text = "Empty Database"
        Button2.Text = "Template Count"
        Button3.Text = "Fingerprint Enroll"
        Button4.Text = "Fingerprint ID"
        NumericUpDown1.Minimum = 0
        NumericUpDown1.Maximum = 255
        Timer1.Interval = 100
        Timer1.Start()
    End Sub

    Private Sub CbbComport_DropDown(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles CbbComport.DropDown
        CbbComport.Items.Clear()
        For i As Integer = 0 To My.Computer.Ports.SerialPortNames.Count - 1
            CbbComport.Items.Add(My.Computer.Ports.SerialPortNames(i))
        Next
    End Sub

    Private Sub BtnConnect_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles BtnConnect.Click
        If BtnConnect.Text = "Connect" Then
            If CbbComport.Text = "" Then
                MsgBox("Please Select COM Port")
            Else
                BtnConnect.Text = "Disconnect"
                CbbComport.Enabled = False

                If SerialPort1.IsOpen Then
                    SerialPort1.Close()
                End If

                SerialPort1.PortName = CbbComport.SelectedItem
                SerialPort1.BaudRate = 9600
                SerialPort1.Parity = IO.Ports.Parity.None
                SerialPort1.StopBits = IO.Ports.StopBits.One
                SerialPort1.DataBits = 8
                SerialPort1.Open()
            End If
        Else
            BtnConnect.Text = "Connect"
            CbbComport.Enabled = True
            SerialPort1.Close()
        End If
    End Sub

    Private Sub Timer1_Tick(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Timer1.Tick
        If SerialPort1.IsOpen Then
            Data1 = SerialPort1.ReadExisting
            If Data1 <> "" Then
                RichTextBox1.AppendText(Data1)
                RichTextBox1.ScrollToCaret()
            End If
        End If
    End Sub

    Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button1.Click
        If SerialPort1.IsOpen Then
            SerialPort1.Write("1")
        End If
    End Sub

    Private Sub Button2_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button2.Click
        If SerialPort1.IsOpen Then
            SerialPort1.Write("2")
        End If
    End Sub

    Private Sub Button3_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button3.Click
        If SerialPort1.IsOpen Then
            SerialPort1.Write("3")
            If NumericUpDown1.Value < 10 Then
                SerialPort1.Write("00")
            ElseIf NumericUpDown1.Value < 100 Then
                SerialPort1.Write("0")
            End If
            SerialPort1.Write(NumericUpDown1.Value)
        End If
    End Sub

    Private Sub Button4_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button4.Click
        If SerialPort1.IsOpen Then
            SerialPort1.Write("4")
        End If
    End Sub
End Class
