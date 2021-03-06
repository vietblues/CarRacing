# CarRacing
This is a project of creating a racing car game. The program is a Winform application which written by C# using Visual Studio IDE

### Game description
_In this game, players need to use the <kbd>A</kbd> and  <kbd>D</kbd> button on keyboard to move the car avoiding be crashed with other cars. For each time of successfully avoiding crashing with another car, players get one point. The game is over when crashing happen. Players can see their points on the game screen. This is an endless runner-style game._ 

### Game screen example

![alt text](https://cloud.githubusercontent.com/assets/11772613/22362808/0e5eda94-e498-11e6-936f-26c61af8ca6b.png "Car Racing Screenshot")

##Programming Flow

### Adding library
```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Media;
```
### Adding variables
```
int _row;
int _col;

int _startX;
int _startY;

int _carX;// the position of car in time
int _carY;// the position of car in time

int _elementSize;
int[,] _gameMatrix;

Random _random;
int _MyCarPosition;

private SoundPlayer _jump;
private SoundPlayer _car;
private SoundPlayer _ding;

int score = 0;
private int newhighscore;
```
### Initialize values
```
  InitializeComponent();
  KeyPreview = true;
  InitializeGame();
  timeRacing.Enabled = false;
  _jump = new SoundPlayer("Jump.wav");
  _car = new SoundPlayer("Door_close.wav");
  _ding = new SoundPlayer("Ding.wav");
  label1.Enabled = false;
```
### InitializeGame
```
 private void InitializeGame()
        {
            _row = 16;
            _col = 6;
            _startX = 50;
            _startY = 25;
            _elementSize = 30;

            _carX = 0;
            _carY = 0;

            _gameMatrix = new int[_row, _col];

            _random = new Random();
            _MyCarPosition = 0;

            DrawACar(12, _MyCarPosition, 2);
       }
  ```
### Create game board
```
 private void Form1_Paint(object sender, PaintEventArgs e)
      {
          DrawGameBoard(e.Graphics);
      }
```
### DrawGameBoard
```
private void DrawGameBoard(Graphics g)
    {
       for(int i = 0; i< _row;i++)
       {
           for(int j = 0; j <_col; j++)
           {
               // draw the grid
               g.DrawRectangle(new Pen(Brushes.Brown), _startX+j*_elementSize,
                   _startY+i*_elementSize,_elementSize,_elementSize);

               // draw another car
               if(_gameMatrix[i,j] == 1)
               {
                   g.FillRectangle(Brushes.DarkCyan,_startX+j*_elementSize,
                   _startY+i*_elementSize,_elementSize,_elementSize);
               }

               //draw my car
               if(_gameMatrix[i,j] == 2)
               {
                   g.FillRectangle(Brushes.DeepSkyBlue,_startX+j*_elementSize,
                   _startY+i*_elementSize,_elementSize,_elementSize);
               }

           }
       }
    }
```
   
### ResetGameBoard()
   ```
   private void ResetGameBoard()
        {
            for (int i = 0; i < _row; i++)
            {
                for (int j = 0; j < _col; j++)
                {
                    _gameMatrix[i, j] = 0;

                }
            }
        }
   ```
   
### DrawACar(int x, int y,int value)
   ```
    private void DrawACar(int x, int y,int value)
        {
            DrawAPoint(x, y + 1, value);
            DrawAPoint(x+1, y + 1, value);
            DrawAPoint(x+2, y + 1, value);
            DrawAPoint(x+3, y + 1, value);
            DrawAPoint(x+1, y, value);
            DrawAPoint(x+1, y + 2, value);
            DrawAPoint(x+3, y, value);
            DrawAPoint(x+3, y + 2, value);
        }
   ```
   
### DrawAPoint(int x, int y, int value)
~~~~
       private void DrawAPoint(int x, int y, int value)
        {
            if (x < _row && x >= 0 && y < _col && y >= 0)
            {
                _gameMatrix[x, y] = value;
            }
        }
~~~~    
 
### Button GO click event
```
private void button1_Click_1(object sender, EventArgs e)
    {
        timeRacing.Interval = 40;
        _carY = _carX = 0;
        _MyCarPosition = 0;
        timeRacing.Enabled = true;
        label1.Enabled = true;
    }
```
    
### timeRacing_Tick function
```
private void timeRacing_Tick(object sender, EventArgs e)
{
    ResetGameBoard();
    DrawACar(12, _MyCarPosition, 2);
    DrawACar(_carX, _carY, 1);
    Invalidate();
    _carX++;
    if (_carX == _row)
    {
        _ding.Play();
        score++;
        label1.Text = Convert.ToString(score);
        _carX = 0;
        var arr1 = new[] { 0, 3};
        _carY = arr1[_random.Next(arr1.Length)];
    }
    CheckGame();
    if (score > 10) timeRacing.Interval = 35;
    if (score > 20) timeRacing.Interval = 34;
    if (score > 30) timeRacing.Interval = 33;
    if (score > 50) timeRacing.Interval = 32;
    if (score > 60) timeRacing.Interval = 31;
    if (score > 70) timeRacing.Interval = 32;
    if (score > 80) timeRacing.Interval = 31;
    if (score > 90) timeRacing.Interval = 32;
    if (score > 100) timeRacing.Interval = 31;
    if (score > 110) timeRacing.Interval = 32;
    if (score > 120) timeRacing.Interval = 31;
 }
```
     
### CheckGame()
```
private void CheckGame()
  {
      if(_carX +3 > 12 && _carY==_MyCarPosition)
      {
          newhighscore = score;
          _car.Play();
          timeRacing.Enabled = false;
          score = 0;
          MessageBox.Show("Game Over");
      }
  }
```
   
### ProcessDialogKey(Keys keyData)
  ```
   protected override bool ProcessDialogKey(Keys keyData)
      {
          switch (keyData)
          {
              case Keys.Left:
              case Keys.A: 
                  if (_MyCarPosition == 3)
                  {
                      _jump.Play();
                      _MyCarPosition = 0;
                  }
                  break;
              case Keys.Right:
              case Keys.D:
                  if (_MyCarPosition == 0)
                  {
                      _jump.Play();
                      _MyCarPosition = 3;
                  }
              return true;
          }
          return base.ProcessDialogKey(keyData);
      }
      ```
        
