#include <ros/ros.h>
#include <stdlib.h>
#include <image_transport/image_transport.h>
#include <opencv2/highgui/highgui.hpp>
#include <cv_bridge/cv_bridge.h>
 
int main(int argc, char** argv)
{
  ros::init(argc, argv, "image_publisher");
  ros::NodeHandle nh;
  image_transport::ImageTransport it(nh);
  image_transport::Publisher pub = it.advertise("camera/image", 1);
 
  
  ros::Rate loop_rate(5);
  for(int i = 0; i < 2000; i++)  
  {
    if(!nh.ok())
        break;
    std::ostringstream stringStream;
    stringStream << "/home/weiyongshnag/camero/" << i << ".jpg";
    cv::Mat image = cv::imread(stringStream.str(), CV_LOAD_IMAGE_COLOR);
    sensor_msgs::ImagePtr msg = cv_bridge::CvImage(std_msgs::Header(), "bgr8", image).toImageMsg();
 
    pub.publish(msg);
    ros::spinOnce();
    loop_rate.sleep();
  }
}
