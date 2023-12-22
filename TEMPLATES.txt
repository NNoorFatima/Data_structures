

#include <iostream>
#include <cstring>
using namespace std;

template <class T>
class vector{
    int size;
    T *ptr;
    public:
    vector(int =10);        //or vector<T>(int=10)
    vector<T>( const vector< T >& );
    ~vector<T>();
    int getsize()const;
    const vector<T>& operator=(const vector<T>&);
    T& operator[](int);
    void print();
};
//====================================
template<class T>
vector<T>::~vector<T>()
{
    delete [] ptr;
}
//====================================
template<class T>
int vector<T>::getsize()const
{
    return size;
}
//====================================
template <class T>
const vector<T>& vector<T>::operator=(const vector<T>& c)
{
    if ( this != &c )   //comparing address 
    {
        delete [] ptr;
        size = c.size;
        if ( size != 0 ) 
        {
            ptr = new T[size];
            for(int i = 0; i < size;i++)
            ptr[i] = c.ptr[i];
        }
        else
        ptr = 0;
    }

    return *this;
}
//====================================
template<class T>
vector<T>::vector(int n)
{
    size=n;
    if(size==0)
        ptr=0;
    else
        ptr= new T[size];
}
//====================================
template<class T>
vector<T>::vector(const vector<T>& c)
{
    if(size==0)
        ptr=0;
    else
    {
        size= c.size;
        ptr= new T[size];
        for(int i=0 ;i<size; i++)
        {
            ptr[i]=c.ptr[i];
        }
    }
}
//====================================
template< class T >
T& vector <T >::operator [](int index ) 
{

    if ( index < 0 || index >= size ) 
    {
        cout << "Error: index out of range\n";
        exit( 1 );
    }
    return ptr[index];
}
//====================================
template< class T >
void vector<T>::print()
{
    if(size!=0)
    {
        for(int i=0; i<size;i++)
            cout<<ptr[i]<<endl;
    }
    
}
// Specialization for char*
template <>
class vector<char*> {
    int size;
    char** ptr;

public:
    vector<char*>(int s)
    {
        size = s;
        if (size != 0) {
            ptr = new char*[size];
            for (int i = 0; i < size; i++) {
                ptr[i] = 0;
            }
        } else
            ptr = 0;
    }

    vector(const vector<char*>&copy)
    {
        size= copy.size;
        if(size == 0)
        {
            ptr=0;
            return;
        }
        ptr= new char*[size];
        for(int i=0 ;i<size ;i++)
        {
            if(copy.ptr[i] !=0)
            {
                ptr[i]=new char[strlen(copy.ptr[i])+1];
                strcpy(ptr[i],copy.ptr[i]);
            }
            else
                ptr[i]=0;
                
        }
    }
    ~vector()
    {
        for(int i=0 ;i<size ;i++)
        {
            delete [] ptr[i];
        }
        delete [] ptr;
    }
    int getsize() const
    {
        return size;
    }
    const vector<char*>& operator=(const vector<char*>&right)
    {
        if(this == & right)//comparing addreses
        {
            return *this;
        }
        for(int i=0 ;i<size ;i++)
            delete [] ptr[i];
            
        delete [] ptr;
        size = right.size;
        ptr= new char*[size];
        for(int i=0 ;i< size ;i++)
        {
            if(right.ptr[i] != 0)
            {
                ptr[i]= new char[strlen(right.ptr[i])+1];
                strcpy(ptr[i],right.ptr[i]);
            }
            else
            ptr[i]=0;
        }
        
    }
    char*& operator[](int index)
    {
        if(index<0 || index>= size)
        {
            cout<<"Error"<<endl;
            exit(1);
        }
        return ptr[index];
    }
    void insert(char* str, int i)
    {
        delete [] ptr[i];
        if ( str != 0 ) {
            ptr[i] = new char[strlen(str)+ 1];
            strcpy( ptr[i], str );
            
        }
        else
            ptr[i] = 0;
    }
    void print()
    {
        for (int i = 0; i < size; i++) 
        {
            for (int j = 0; ptr[i][j] != '\0'; j++) 
            {
                cout << ptr[i][j] << "";
            }
        cout << endl;
    }
        
    }
};


int main()
{
    vector <int> i(2);
    i[0]=34;
    i[1]=23;
    i.print();
    vector <float> e(2);
    e[0]=0.234;
    e[1]=2.3;
    e.print();
    vector <char> k(2);
    k[0]='s';
    k[1]='2';
    k.print();
    vector<char*> l(2);
   // char *am="aamri";
    l[0]="aamri";
 //   l.insert("amir",0);
   // l.insert("addar",1);
    l.print();
    return 0;
}
