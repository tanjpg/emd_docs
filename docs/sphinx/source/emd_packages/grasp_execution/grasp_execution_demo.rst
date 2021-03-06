.. easy_manipulation_deployment documentation master file, created by
   sphinx-quickstart on Thu Oct 22 11:03:35 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. _grasp_execution:

Grasp Execution
========================================================

Overview
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Easy Manipulation Deployment Grasp Execution package was developed to provide a robust path planning process to navigate robot to the taget location for grasping. The package serves as a grasp execution simulator using Moveit2 path planners and the results from the :ref:`grasp_planner`.

.. note:: It is recommended that you use scene packages generated by the :ref:`workcell_builder`. If you are using a robot, make sure you have the **moveit config folder** (check out :ref:`workcell_initialization` for more information about the moveit_config folder)

Benefits of EMD Grasp Execution
---------------------------------------------------------------------

**1. Seamless intergration with EMD Grasp Planner**

The EMD Grasp Execution package communicates with the Grasp Planner package through the subscription to a single ROS2 topic with the GraspTask.msg type. Details of the GraspTask Message can be found here: :ref:`grasp_planner_output`

**2. Dynamic Safety Capabilities**

In real life use cases, collaborative robots often operate closely with human operators or reside in an ever-changing environment. There is thus a need for the robot to be equipped with the dynamic safety capability, to detect possible collision during its trajectory execution and avoid these occurring obstacles.

Grasp Execution provides users with a vision based dynamic collision avoidance capability using Octomaps. When a collision has been deemed to occur in the trajectory of the robot, the dynamic safety module will be triggered. This would either stop the robot to avoid collision, or call for the dynamic replanning of its trajectory given the new collision objects in the scene.

Package configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Before running the grasp execution package, make sure that you have a scene package in the ``workcell/src/scenes/`` folder.

Launch file
-------------

Open the ``grasp_execution.launch.py`` file in ``/workcell/src/easy_manipulation_deployment/grasp_execution/example/launch/`` and change the parameters accordingly.

**scene_pkg**

The name of your scene package

Example: 

.. code-block:: bash

   scene_pkg = 'ur5_2f_test'

**base_link**

The base link of the robot connected to the table surface 

Example: 

.. code-block:: bash

   robot_base_link = 'base_link'

grasp_execution_node.cpp
--------------------------

Open the ``demo_node.cpp`` file in ``/workcell/src/easy_manipulation_deployment/grasp_execution/example/src/`` and change the parameters accordingly.

.. code-block:: bash

   static const char PLANNING_GROUP[] = "manipulator";

   static const char EE_LINK[] = "ee_palm";

   static const float CLEARANCE = 0.1;

   static const char GRASP_TASK_TOPIC[] = "grasp_tasks";

**PLANNING_GROUP**

Robot group state that is declared in the srdf. for example, in the file ``ur5_moveit_config/config/ur5.srdf.xacro``:

.. code-block:: bash

    <!--GROUPS: Representation of a set of joints and links. This can be useful for specifying DOF to plan for, defining arms, end effectors, etc-->
    <group name="manipulator">
        <chain base_link="base_link" tip_link="ee_link" />
    </group>

the planning_group is then *manipulator*

**EE_LINK**

Tip link of end effector

**CLEARANCE**

Distance above the object that the end effector would plan to before moving down to pick the object up

**GRASP_TASK_TOPIC**

ROS2 topic from the Grasp Planning component that will output the GraspTask message

Addidional configurations
--------------------------

Other configurations that can be customized for grasp execution

.. toctree::
   :maxdepth: 2

   grasp_execution_configuration


Running grasp execution
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   ros2 launch grasp_execution grasp_execution.launch.py

You should then see RViz launch with the initial workcell scene, where the robot arm is at its home state.

.. note:: Ensure that on your terminal, the ROS2 distribution is sourced, Moveit2 is sourced, and the workspace is built and sourced as well.

Publishing a Fake Task
--------------------------

.. code-block:: bash

   ros2 launch grasp_execution fake_task_publisher.launch.xml

This publishes an object for the robot to conduct a pick-and-place operation. The location and dimensions of the object can be configured in ``grasp_execution/example/config/fake_grasp_pose_publisher.yaml`` as described in the :ref:`configuration page<grasp_execution_configuration>`.
