# UTS-Pengolahan-Citra
function varargout = Latihan01(varargin)
% Latihan01 MATLAB code for Latihan01.fig
%      Latihan01, by itself, creates a new Latihan01 or raises the existing
%      singleton*.
%
%      H = Latihan01 returns the handle to a new Latihan01 or the handle to
%      the existing singleton*.
%
%      Latihan01('CALLBACK',hObject,eventData,handles,...) calls the local
%      function named CALLBACK in Latihan01.M with the given input arguments.
%
%      Latihan01('Property','Value',...) creates a new Latihan01 or raises the
%      existing singleton*.  Starting from the left, property value pairs are
%      applied to the GUI before Latihan01_OpeningFcn gets called.  An
%      unrecognized property name or invalid value makes property application
%      stop.  All inputs are passed to Latihan01_OpeningFcn via varargin.
%
%      *See GUI Options on GUIDE's Tools menu.  Choose "GUI allows only one
%      instance to run (singleton)".
%
% See also: GUIDE, GUIDATA, GUIHANDLES

% Edit the above text to modify the response to help Latihan01

% Last Modified by GUIDE v2.5 08-May-2021 01:09:42

% Begin initialization code - DO NOT EDIT
gui_Singleton = 1;
gui_State = struct('gui_Name',       mfilename, ...
                   'gui_Singleton',  gui_Singleton, ...
                   'gui_OpeningFcn', @Latihan01_OpeningFcn, ...
                   'gui_OutputFcn',  @Latihan01_OutputFcn, ...
                   'gui_LayoutFcn',  [] , ...
                   'gui_Callback',   []);
if nargin && ischar(varargin{1})
    gui_State.gui_Callback = str2func(varargin{1});
end

if nargout
    [varargout{1:nargout}] = gui_mainfcn(gui_State, varargin{:});
else
    gui_mainfcn(gui_State, varargin{:});
end
% End initialization code - DO NOT EDIT


% --- Executes just before Latihan01 is made visible.
function Latihan01_OpeningFcn(hObject, eventdata, handles, varargin)
% This function has no output args, see OutputFcn.
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
% varargin   command line arguments to Latihan01 (see VARARGIN)

% Choose default command line output for Latihan01
handles.output = hObject;

% Update handles structure
guidata(hObject, handles);

% UIWAIT makes Latihan01 wait for user response (see UIRESUME)
% uiwait(handles.figure1);


% --- Outputs from this function are returned to the command line.
function varargout = Latihan01_OutputFcn(hObject, eventdata, handles) 
% varargout  cell array for returning output args (see VARARGOUT);
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Get default command line output from handles structure
varargout{1} = handles.output;


% --- Executes on button press in btnopen.
function btnopen_Callback(hObject, eventdata, handles)

appcitra = guidata(gcbo);
[namafile, formatfile]=uigetfile({'*.jpg';'*.png';'*.bmp'},'tampil gambar');

gambar=imread(namafile);
guidata(hObject, handles);
axes(handles.citra_asli);
imshow (gambar), title('Picture');

set(appcitra.figure1, 'userdata', gambar);
set(appcitra.citra_asli, 'userdata', gambar);

% --- Executes on button press in btnproses.
function btnproses_Callback(hObject, eventdata, handles)
appcitra=guidata(gcbo);
I = get(appcitra.citra_asli, 'userdata');
if isequal(I,[])
    msgbox('Masukkan Gambar !','Peringatan','Warm');
else
    %TrueColor%
    negative = 255-1 -I;
    set(appcitra.figure1, 'CurrentAxes',appcitra.axes1_hasil);
    set(imshow(negative));
    imshow(negative),title('Citra True Color');
    
    %Histogram Negative%
    axes(handles.axes1_histo)
    x=1:256;
    plot(x,imhist(negative(:,:,1)), 'r-');
    hold on;
    plot(x, imhist(negative(:,:,2)), 'g-');
    plot(x, imhist(negative(:,:,3)), 'b-');
    title('Histrogram Negative')
        
end
% --- Executes on button press in btnsimpanbin.
function btnsimpanbin_Callback(hObject, eventdata, handles)
[namafile, direktori]=uiputfile({'*.jpg';'*.png'}, 'Save Image As');
sG = getimage(handles.axes1_hasil);
save = [direktori namafile];
imwrite(sG, save, 'jpg');
msgbox('foto berhasil disimpan ! ','sukses');
