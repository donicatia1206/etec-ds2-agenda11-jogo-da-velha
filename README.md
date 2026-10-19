# ❌⭕ Jogo da Velha em .NET MAUI

Atividade da **Agenda 11** — Desenvolvimento de Sistemas II (DS2)  
**Centro Paula Souza - ETEC**  
**Aluno:** Donizete dos Santos  
**Professor Mediador:** Tadeu Fernando Piran  

---

## 📌 Descrição do Projeto

Aplicativo móvel do **Jogo da Velha** desenvolvido em **.NET MAUI (C# e XAML)**. 
O projeto foca na personalização da interface gráfica utilizando gradiente de cores, estilização reutilizável com Styles e Setters, organização responsiva via Grid e manipulação de eventos com o parâmetro `sender`.

---

## 🛠️ Tecnologias e Conceitos Aplicados

- **Plataforma:** .NET 8.0 MAUI
- **Linguagens:** XAML (Interface) e C# (Lógica de Negócio)
- **Background Degradê (`LinearGradientBrush`):** Gradiente linear para um visual moderno.
- **Layout em Grade (`Grid`):** Organização em 3 linhas e 3 colunas para o tabuleiro de botões e cabeçalho mesclado com `Grid.ColumnSpan`.
- **Estilos Globais (`Style` e `Setter`):** Padronização visual dos botões (cores, bordas e cantos arredondados) sem repetição de código.
- **Manipulador de Eventos (`object sender`):** Identificação dinâmica do botão clicado para alternância de jogadas ('X' e 'O') e verificação de condições de vitória.

---

## 📹 Vídeo de Demonstração e Explicativo do Código

🎥 **Assista ao vídeo explicativo:**  
`[https://youtu.be/px7wMYtMRFY?si=_oLHXBPd3caNOwWH]`

---

## 💻 Código-Fonte da Aplicação

### Interface Visual (`MainPage.xaml`)

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage x:Class="MauiAppJogoDaVelha.MainPage" xmlns="[http://schemas.microsoft.com/dotnet/2021/maui](http://schemas.microsoft.com/dotnet/2021/maui)" xmlns:x="[http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)">

    <ContentPage.Background>
        <LinearGradientBrush EndPoint="1,1" StartPoint="0,0">
            <GradientStop Color="#00C9FF" Offset="0.0"/>
            <GradientStop Color="#92FE9D" Offset="1.0"/>
        </LinearGradientBrush>
    </ContentPage.Background>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Button">
                <Setter Property="BackgroundColor" Value="#10FFFFFF"/>
                <Setter Property="CornerRadius" Value="20"/>
                <Setter Property="TextColor" Value="#1A252C"/>
                <Setter Property="FontSize" Value="60"/>
                <Setter Property="FontAttributes" Value="Bold"/>
                <Setter Property="BorderWidth" Value="2"/>
                <Setter Property="BorderColor" Value="#FFFFFF"/>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <Grid ColumnDefinitions="*, *, *" ColumnSpacing="12" Margin="20" RowDefinitions="Auto, *, *, *" RowSpacing="12">

        <Label FontAttributes="Bold" FontSize="32" Grid.Column="0" Grid.ColumnSpan="3" Grid.Row="0" HorizontalOptions="Center" Margin="0,20,0,10" Text="Jogo da Velha" TextColor="#1A252C" VerticalOptions="Center"/>

        <!-- Linha 1 -->
        <Button Clicked="Button_Clicked" Grid.Column="0" Grid.Row="1" x:Name="btn10"/>
        <Button Clicked="Button_Clicked" Grid.Column="1" Grid.Row="1" x:Name="btn11"/>
        <Button Clicked="Button_Clicked" Grid.Column="2" Grid.Row="1" x:Name="btn12"/>

        <!-- Linha 2 -->
        <Button Clicked="Button_Clicked" Grid.Column="0" Grid.Row="2" x:Name="btn20"/>
        <Button Clicked="Button_Clicked" Grid.Column="1" Grid.Row="2" x:Name="btn21"/>
        <Button Clicked="Button_Clicked" Grid.Column="2" Grid.Row="2" x:Name="btn22"/>

        <!-- Linha 3 -->
        <Button Clicked="Button_Clicked" Grid.Column="0" Grid.Row="3" x:Name="btn30"/>
        <Button Clicked="Button_Clicked" Grid.Column="1" Grid.Row="3" x:Name="btn31"/>
        <Button Clicked="Button_Clicked" Grid.Column="2" Grid.Row="3" x:Name="btn32"/>

    </Grid>

</ContentPage>

using System;
using Microsoft.Maui.Controls;

namespace MauiAppJogoDaVelha
{
    public partial class MainPage : ContentPage
    {
        string vez = "X";

        public MainPage()
        {
            InitializeComponent();
        }

        private void Button_Clicked(object sender, EventArgs e)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            if (vez == "X")
            {
                btn.Text = "X";
                vez = "O";
            }
            else
            {
                btn.Text = "O";
                vez = "X";
            }

            VerificarVitoria();
        }

        private void VerificarVitoria()
        {
            if (ChecarTripla(btn10, btn11, btn12) ||
                ChecarTripla(btn20, btn21, btn22) ||
                ChecarTripla(btn30, btn31, btn32) ||
                ChecarTripla(btn10, btn20, btn30) ||
                ChecarTripla(btn11, btn21, btn31) ||
                ChecarTripla(btn12, btn22, btn32) ||
                ChecarTripla(btn10, btn21, btn32) ||
                ChecarTripla(btn12, btn21, btn30))
            {
                string vencedor = (vez == "X") ? "O" : "X";
                DisplayAlert("Parabéns!", $"O jogador '{vencedor}' venceu!", "OK");
                Zerar();
            }
        }

        private bool ChecarTripla(Button b1, Button b2, Button b3)
        {
            return !string.IsNullOrEmpty(b1.Text) && 
                   b1.Text == b2.Text && 
                   b2.Text == b3.Text;
        }

        private void Zerar()
        {
            Button[] botoes = { btn10, btn11, btn12, btn20, btn21, btn22, btn30, btn31, btn32 };
            foreach (var btn in botoes)
            {
                btn.Text = "";
                btn.IsEnabled = true;
            }
            vez = "X";
        }
    }
}
