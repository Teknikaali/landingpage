---
title: "Tehdään WPF-sovellus - 29 - Sukupuolen valinta"
image: assets/images/tehdaan-wpf-sovellus/ketuttaako-header.jpg
layout: post
date: 2021-08-07 12:00
tag:
- WPF
category: blog
author: anssikettunen
description: Opas kuinka tehdä Windows-työpöytäsovellus käyttäen WPF, XAML ja C#. Tässä osassa laskuriin lisätään sukupuolen valinta.
previous: hidden
---

## Toiminnon haasteet

BMI:n laskukaava ei ota huomioon sitä, että naisten ja miesten kehot eivät ole sama asia.
En lyötänyt googlettelamalla suoraa kaavaa, jolla voisi helposti erottaa näiden kahden sukupuolen välisiä eroavaisuuksia. Kovasti löytyi vain graafeja ja sepustuksia, juttua rasvaprosenteista, kehon rakenteen vaikutuksista (lihasten määrä jne.) ja onko henkilö raskaana vai ei.
Tehdään kuitenkin parhaamme asian eteen!

Päädyin käyttämään Wikipediasta löytyvää vuonna 2013 ehdotettua ["uutta laskukaavaa"](https://en.wikipedia.org/wiki/Body_mass_index#Proposed_new_BMI).

## Valintapainikkeiden lisäys käyttöliittymään

Aloitetaan lisäämällä uusi rivi valintapainikkeille ja annetaan niille myös eri symbolit kertomaan valinnasta.

1) Lisätään `InputView.xaml`-näkymän ylimmälle Gridille yksi `RowDefinition` lisää.
2) Siirretään painon ja pituuden osuudet yhden rivin verran alemmas (`Grid.Row` 0 -> 1 ja 1 -> 2)
3) Lisätään sukupuolen valintapainikkeet omaan ruudukkoonsa painon ja pituuden yläpuolelle
4) Esitetään valintapainikkeet `RadioButton`-tyyppisinä painikkeina

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition />
        <RowDefinition />
        <RowDefinition />
    </Grid.RowDefinitions>

    <!--  Gender  -->
    <Grid Margin="24,24,24,0">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <RadioButton Margin="0,0,4,0"
                     Content="FEMALE"
                     GroupName="Gender" />

        <RadioButton Grid.Column="1"
                     Margin="4,0,0,0"
                     Content="MALE"
                     GroupName="Gender" />
    </Grid>

    <!--  Weight  -->
```
<figcaption>InputView.xaml</figcaption>

![][1]{: .center-image }
<figcaption class="caption">Ensimmäinen versio valintapainikkeista</figcaption>

`RadioButton.GroupName` kertoo ryhmän nimen (tässä tapauksessa sama molemmille), jonka sisällä painikkeiden `IsChecked`-arvoa tarkastellaan. [Täällä](https://docs.microsoft.com/en-us/dotnet/api/system.windows.controls.radiobutton.groupname) lisää aiheesta.
Tarvitsemme myös `RadioButton.IsChecked`-arvon bindauksen ViewModelin puolelle, mutta tämä vaatii hieman lisätyötä toimiakseen oikein.
Tyylitellään painikkeet ensin.

## Valintapainikkeiden tyylittely

Emme halua valintapainikkeiden pienten ympyröiden näkyvän ja haluaisimme myös näyttää sukupuolen symbolit painikkeissa.
Toteutetaan oma [`ControlTemplate`](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/controls/how-to-create-apply-template), jonka avulla voimme muokata nykyisen `RadioButton`-kontrollin ulkoasua haluamaksemme.

Lisätään siis seuraavaksi `RadioButton`:lle tyylimäärittely `App.xaml`-tiedostoon heti `Border`-tyylin jälkeen:

```xml
<Style TargetType="RadioButton">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="{x:Type RadioButton}">
                <Border Style="{StaticResource ItemBorder}">
                    <Grid Margin="10">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="2*" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <Viewbox>
                            <TextBlock Name="Symbol"
                                       Text="{TemplateBinding Tag}" />
                        </Viewbox>
                        <TextBlock Name="Description"
                                   Grid.Row="1"
                                   Text="{TemplateBinding Content}" />
                    </Grid>
                </Border>
                <ControlTemplate.Triggers>
                    <Trigger Property="IsChecked" Value="True">
                        <Setter TargetName="Description" Property="Foreground" Value="White" />
                        <Setter TargetName="Symbol" Property="Foreground" Value="White" />
                    </Trigger>
                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```
<figcaption>App.xaml</figcaption>

Edeltävässä tyylissä on seuraavat erikoisuudet, joita emme ole aiemmin käsitelleet:
- Symbolin tekstikenttä on [`ViewBox`](https://www.dotnetperls.com/viewbox-wpf)-elementin sisällä, jotta symboli skaalautuisi rivin korkeuden mukaan huolimatta siitä, että se on tekstiä, jolla on jo valmiiksi määritetty fonttikoko.
- Käytämme [`TemplateBinding`](https://www.codeproject.com/Tips/599954/WPF-TemplateBinding-with-ControlTemplate)-bindausta `RadioButton.Tag`- ja `.Content`-arvojen bindaamiseen. Tämä mahdollistaa sen, että kun annamme näille arvot näkymän puolella, niin annetut arvot näytetään kyseisissä kohdissa tyyliä.
- Lisäämällä triggerin `ControlTemplate.Triggers`-kokoelmaan saamme aktiivisena olevan valinnan näkymään kirkkaampana.

Nyt voimme käydä lisäämässä symbolit painikkeiden `Tag`-arvoihin..

```xml
<RadioButton Tag="♀" ...
```
<figcaption>InputView.xaml</figcaption>

..ja lopputuloksen tulisi näyttää jotakuinkin tältä:

![][2]{: .center-image }
<figcaption class="caption">Symbolit ja tyyli lisätty</figcaption>

## ViewModeliin bindaaminen ja Converter

Jotta voisimme vaihtaa käytettävää laskukaavaa riippuen sukupuolen valinnasta, niin meidän tulee lisätä `InputViewModel`:lle uusi property `Gender`.
Luodaan samalla uusi `Gender`-enum.

```csharp
public enum Gender
{
    Female,
    Male
}
```
<figcaption>Gender.cs</figcaption>

```csharp
private Gender _gender;
public Gender Gender
{
    get => _gender;
    set => SetProperty(ref _gender, value);
}
```
<figcaption>InputViewModel.cs</figcaption>

Olisimme luultavasti pärjänneet yksinkertaisella `bool`-arvolla (`IsFemale`), mutta koska emme halua rajata vaihtoehtojen lukumäärää, niin katsoin enumin sopivan tähän tarkoitukseen paremmin.
Vastassamme on kuitenkin ongelma. [`RadioButton.IsChecked`](https://docs.microsoft.com/en-us/dotnet/api/system.windows.controls.primitives.togglebutton.ischecked) on tyypiltään `bool?`. `Gender`-tyyppiä ei voi siis suoraan bindata eri tyyppiseen arvoon, vaan meidän täytyy _konvertoida_ arvot sopiviksi.
Meillä on edessämme ensimmäisen [Converterimme](https://www.wpf-tutorial.com/data-binding/value-conversion-with-ivalueconverter/) tekeminen. Nice.

Lisätään uusi konverteri `GenderConverter`:

```csharp
public class GenderConverter : MarkupExtension, IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        => (Gender)value == (Gender)parameter;

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
        => (bool?)value == true
            ? parameter
            : null;

    public override object ProvideValue(IServiceProvider serviceProvider)
        => this;
}
```
<figcaption>GenderConverter.cs</figcaption>

`GenderConverter` perii `MarkupExtensionin` ja toteuttaa `IValueConverterin. Tästä lisää [täällä](http://www.thejoyofcode.com/WPF_Quick_Tip_Converters_as_MarkupExtensions.aspx).
`MarkupExtension` sallii käytännössä meidän käyttää tätä konverteria lisäämättä sitä erikseen näkymän resursseihin. Kätevää!

Käytännössä siis tämä konverteri ottaa vastaan `Gender`-tyyppisen arvon ja jos se on sama kuin parametrina annettu `Gender`, niin palautetaan `true`. Jos tästä `Convert`-funktiosta palautetaan `true`, niin kyseisen painikkeen `IsChecked` asetetaan todeksi.
Sama tapahtuu `ConvertBack`-funktiossa, mutta vain toisin päin. Eli sisään tulee `bool?` ja ulos annetaan `Gender`.

Nyt voimme mennä rakentamaan bindauksen näkymän puolelle.
Voimme antaa `ConverterParametriksi` suoraan `Gender`-enumin arvon käyttämällä `x:Static`-avainsanaa, jolloin vältymme vertailemasta esim. `string`-tyyppisiä arvoja konverterissamme (esim. jos antaisimme parametrin yksinkertaisemmin näin: `ConverterParameter=Female`).

```xml
<RadioButton ...
             IsChecked="{Binding Gender, Converter={local:GenderConverter}, ConverterParameter={x:Static local:Gender.Female}}"
             Tag="♀" />
<RadioButton ...
             IsChecked="{Binding Gender, Converter={local:GenderConverter}, ConverterParameter={x:Static local:Gender.Male}}"
             Tag="♂" />
```
<figcaption>InputView.xaml</figcaption>

![][3]{: .center-image }
<figcaption class="caption">Bindaukset bindattu</figcaption>

## Laskurin vaihtaminen sukupuolen perusteella

Tehdään seuraavat asiat `Calculator.cs`-tiedostossa ja lisätään uusi laskuri samalla:
1) Luodaan uusi rajapinta `ICalculator`, joka helpottaa laskennan suorittamista jatkossa.
2) Siirretään `Calculator`-luokalta kommentit `ICalculator`-rajapinnalle
3) Nimetään alkuperäinen laskurimme `Calculator` -> `MaleCalculator`
4) Lisätään uusi `FemaleCalculator`

Tämä tyyli mahdollistaa meidän lisätä eri kaavoilla laskevia laskureita suhteellisen vähällä vaivalla jatkossa.

Muokattu `Calculator.cs`-tiedostomme näyttää muutosten jälkeen tältä:

```csharp
/// <summary>
/// Provides a method to calculate a body mass index (BMI) value for a person.
/// </summary>
public interface ICalculator
{
    /// <summary>
    /// Calculates body mass index (BMI) derived from mass (weight) and height of a person.
    /// </summary>
    /// <param name="weight">Weight in kilograms</param>
    /// <param name="height">Height in meters</param>
    /// <returns>Body mass index value in units of kg/m²</returns>
    double Calculate(double weight, double height);
}

public class MaleCalculator : ICalculator
{
    public double Calculate(double weight, double height) => weight / Math.Pow(height, 2);
}

public class FemaleCalculator : ICalculator
{
    public double Calculate(double weight, double height) => 1.3 * weight / Math.Pow(height, 2.5);
}
```
<figcaption>Calculator.cs</figcaption>

Joudumme tämän myötä tekemään hieman muutoksia `MainViewModel`-luokankin puolelle.
Meidän tulee päättää `InputViewModel.Gender`-tiedon perusteella, että mitä laskuria haluamme käyttää kyseiselle valinnalle.

Toteutetaan valinta siten, että korvataan nykyinen `private readonly Calculator _calculator;` `Dictionary`-tyyppisellä ratkaisulla.
Tämä mahdollistaa sen, että voimme hakea `Dictionarysta` avaimella (`Gender`) halutun `ICalculator`-instanssin:

```diff
-private readonly Calculator _calculator;
+private readonly IDictionary<Gender, ICalculator> _calculators;

...

-_calculator = new Calculator();
+_calculators = new Dictionary<Gender, ICalculator>
+{
+    { Gender.Female, new FemaleCalculator() },
+    { Gender.Male, new MaleCalculator() }
+};

...

-_resultViewModel.Result = _calculator.Calculate(input.Weight, input.Height / 100);
+_resultViewModel.Result = _calculators[input.Gender].Calculate(input.Weight, input.Height / 100);
```

Katsotaan mitä saimme aikaiseksi:

![][4]{: .center-image }
<figcaption class="caption">Eri laskurien tulokset</figcaption>

Siistiä! 🦊

[1]: /assets/images/tehdaan-wpf-sovellus/29-01.jpg
[2]: /assets/images/tehdaan-wpf-sovellus/29-02.jpg
[3]: /assets/images/tehdaan-wpf-sovellus/29-03.jpg
[4]: /assets/images/tehdaan-wpf-sovellus/29-04.gif